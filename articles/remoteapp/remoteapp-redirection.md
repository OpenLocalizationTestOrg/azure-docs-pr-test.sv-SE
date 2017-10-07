---
title: aaaUsing omdirigering i Azure RemoteApp | Microsoft Docs
description: "Lär dig hur tooconfigure och använda omdirigering i RemoteApp"
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 2c8c867f-4907-4f2e-9ccd-2eb82bb5b837
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: d5739a75cf606bd971268da67b2c5ff0fe5fe19b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a><span data-ttu-id="1419a-103">Med hjälp av omdirigering i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="1419a-103">Using redirection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1419a-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="1419a-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="1419a-105">Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.</span><span class="sxs-lookup"><span data-stu-id="1419a-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="1419a-106">Omdirigering av enheter gör att användare kan interagera med fjärråtkomst appar som använder hello enheter anslutna tootheir lokala datorn, telefon eller surfplatta.</span><span class="sxs-lookup"><span data-stu-id="1419a-106">Device redirection lets your users interact with remote apps using hello devices attached tootheir local computer, phone, or tablet.</span></span> <span data-ttu-id="1419a-107">Om du har angett Skype via Azure RemoteApp, måste användaren hello kamera installerad på sin dator toowork med Skype.</span><span class="sxs-lookup"><span data-stu-id="1419a-107">For example, if you have provided Skype through Azure RemoteApp, your user needs hello camera installed on their PC toowork with Skype.</span></span> <span data-ttu-id="1419a-108">Detta gäller även för skrivare, högtalare, Övervakare och ett intervall av USB-anslutna kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="1419a-108">This is also true for printers, speakers, monitors, and a range of USB-connected peripherals.</span></span>

<span data-ttu-id="1419a-109">RemoteApp utnyttjar hello Remote Desktop Protocol (RDP) och RemoteFX tooprovide omdirigering.</span><span class="sxs-lookup"><span data-stu-id="1419a-109">RemoteApp leverages hello Remote Desktop Protocol (RDP) and RemoteFX tooprovide redirection.</span></span>

## <a name="what-redirection-is-enabled-by-default"></a><span data-ttu-id="1419a-110">Vilka omdirigering är aktiverat som standard?</span><span class="sxs-lookup"><span data-stu-id="1419a-110">What redirection is enabled by default?</span></span>
<span data-ttu-id="1419a-111">När du använder RemoteApp är hello följande omdirigeringar aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="1419a-111">When you use RemoteApp, hello following redirections are enabled by default.</span></span> <span data-ttu-id="1419a-112">hello information inom parentes visar hello RDP-inställningen.</span><span class="sxs-lookup"><span data-stu-id="1419a-112">hello information in parentheses show hello RDP setting.</span></span>

* <span data-ttu-id="1419a-113">Spela upp ljud på hello lokal dator (**spela upp på den här datorn**).</span><span class="sxs-lookup"><span data-stu-id="1419a-113">Play sounds on hello local computer (**Play on this computer**).</span></span> <span data-ttu-id="1419a-114">(audiomode:i:0)</span><span class="sxs-lookup"><span data-stu-id="1419a-114">(audiomode:i:0)</span></span>
* <span data-ttu-id="1419a-115">Spela in ljud från hello lokala datorn och skicka toohello fjärrdatorn (**poster från den här datorn**).</span><span class="sxs-lookup"><span data-stu-id="1419a-115">Capture audio from hello local computer and send toohello remote computer (**Record from this computer**).</span></span> <span data-ttu-id="1419a-116">(audiocapturemode:i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-116">(audiocapturemode:i:1)</span></span>
* <span data-ttu-id="1419a-117">Skriva ut toolocal skrivare (redirectprinters:i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-117">Print toolocal printers (redirectprinters:i:1)</span></span>
* <span data-ttu-id="1419a-118">COM-portar (redirectcomports:i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-118">COM ports (redirectcomports:i:1)</span></span>
* <span data-ttu-id="1419a-119">Smartkortenhet (redirectsmartcards:i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-119">Smart card device (redirectsmartcards:i:1)</span></span>
* <span data-ttu-id="1419a-120">Urklipp (möjlighet toocopy och klistra in) (redirectclipboard:i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-120">Clipboard (ability toocopy and paste) (redirectclipboard:i:1)</span></span>
* <span data-ttu-id="1419a-121">Avmarkera typen kantutjämning (Tillåt kantutjämning: i:1)</span><span class="sxs-lookup"><span data-stu-id="1419a-121">Clear type font smoothing (allow font smoothing:i:1)</span></span>
* <span data-ttu-id="1419a-122">Dirigera alla kompatibla Plug and Play-enheter.</span><span class="sxs-lookup"><span data-stu-id="1419a-122">Redirect all supported Plug and Play devices.</span></span> <span data-ttu-id="1419a-123">(devicestoredirect:s: *)</span><span class="sxs-lookup"><span data-stu-id="1419a-123">(devicestoredirect:s:*)</span></span>

## <a name="what-other-redirection-is-available"></a><span data-ttu-id="1419a-124">Vilka andra omdirigering är tillgängligt?</span><span class="sxs-lookup"><span data-stu-id="1419a-124">What other redirection is available?</span></span>
<span data-ttu-id="1419a-125">Två alternativ för mappomdirigering är inaktiverade som standard:</span><span class="sxs-lookup"><span data-stu-id="1419a-125">Two redirection options are disabled by default:</span></span>

* <span data-ttu-id="1419a-126">Omdirigering (enhetsmappning): den lokala datorn enheter bli mappade enheter i hello fjärrsession.</span><span class="sxs-lookup"><span data-stu-id="1419a-126">Drive redirection (drive mapping): Your local computer's drives become mapped drives in hello remote session.</span></span> <span data-ttu-id="1419a-127">Detta kan du spara eller öppna filer från de lokala enheterna när du arbetar i hello fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="1419a-127">This lets you save or open files from your local drives while you work in hello remote session.</span></span>
* <span data-ttu-id="1419a-128">USB-omdirigering: du kan använda hello USB-enheter anslutna tooyour lokal dator inom hello fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="1419a-128">USB redirection: You can use hello USB devices attached tooyour local computer within hello remote session.</span></span>

## <a name="change-your-redirection-settings-in-remoteapp"></a><span data-ttu-id="1419a-129">Ändra inställningarna för omdirigering i RemoteApp</span><span class="sxs-lookup"><span data-stu-id="1419a-129">Change your redirection settings in RemoteApp</span></span>
<span data-ttu-id="1419a-130">Du kan ändra hello enhetens omdirigeringsinställningar för en samling med hello Microsoft Azure PowerShell med SDK.</span><span class="sxs-lookup"><span data-stu-id="1419a-130">You can change hello device redirection settings for a collection by using hello Microsoft Azure PowerShell with SDK.</span></span> <span data-ttu-id="1419a-131">När du har installerat Hej nya PowerShell och SDK, först konfigurera din prenumeration som beskrivs i toomanage [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1419a-131">After you install hello new PowerShell and SDK, first configure it toomanage your subscription as described in [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="1419a-132">Använd sedan en liknande toohello för kommandot efter tooset hello anpassade RDP egenskaper:</span><span class="sxs-lookup"><span data-stu-id="1419a-132">Then use a command similar toohello following tooset hello custom RDP properties:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="1419a-133">(Observera att  *\`n*  används som avgränsare mellan enskilda egenskaper.)</span><span class="sxs-lookup"><span data-stu-id="1419a-133">(Note that *\`n* is used as a delimiter between individual properties.)</span></span>

<span data-ttu-id="1419a-134">tooget en lista över vilka anpassade RDP-egenskaper har konfigurerats, kör hello följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="1419a-134">tooget a list of what custom RDP properties are configured, run hello following cmdlet.</span></span> <span data-ttu-id="1419a-135">Observera att endast anpassade egenskaper visas som utdataresultat och inte hello standardegenskaperna:</span><span class="sxs-lookup"><span data-stu-id="1419a-135">Note that only custom properties are shown as output results and not hello default properties:</span></span>  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

<span data-ttu-id="1419a-136">När du ställer in egenskaperna måste du ange alla anpassade egenskaper varje gång; Annars återställs hello inställningen toodisabled.</span><span class="sxs-lookup"><span data-stu-id="1419a-136">When you set custom properties you must specify all custom properties each time; otherwise hello setting reverts toodisabled.</span></span>   

### <a name="common-examples"></a><span data-ttu-id="1419a-137">Vanliga exempel</span><span class="sxs-lookup"><span data-stu-id="1419a-137">Common examples</span></span>
<span data-ttu-id="1419a-138">Använd följande cmdlet tooenable omdirigering hello:</span><span class="sxs-lookup"><span data-stu-id="1419a-138">Use hello following cmdlet tooenable drive redirection:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

<span data-ttu-id="1419a-139">Använd den här cmdlet-tooenable omdirigering av såväl USB-enhet:</span><span class="sxs-lookup"><span data-stu-id="1419a-139">Use this cmdlet tooenable both USB and Drive redirection:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="1419a-140">Använd denna cmdlet toodisable delning av Urklipp:</span><span class="sxs-lookup"><span data-stu-id="1419a-140">Use this cmdlet toodisable clipboard sharing:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> <span data-ttu-id="1419a-141">Vara att toocompletely logga ut alla användare i samlingen hello (och inte bara koppla från dem) innan du testar hello ändringen.</span><span class="sxs-lookup"><span data-stu-id="1419a-141">Be sure toocompletely log off all users in hello collection (and not just disconnect them) before you test hello change.</span></span> <span data-ttu-id="1419a-142">tooensure användare har helt loggat ut gå toohello **sessioner** i hello-samlingen i hello Azure-portalen och logga ut alla användare som har kopplats från eller loggat in.</span><span class="sxs-lookup"><span data-stu-id="1419a-142">tooensure users are completely logged off, go toohello **Sessions** tab in hello collection in hello Azure portal and log off any users who are disconnected or signed in.</span></span> <span data-ttu-id="1419a-143">Ibland kan det ta flera sekunder innan hello lokala enheter tooshow i Explorer hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="1419a-143">Sometimes it can take several seconds for hello local drives tooshow in Explorer within hello session.</span></span>
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a><span data-ttu-id="1419a-144">Ändra inställningar för USB-omdirigering på Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="1419a-144">Change USB redirection settings on your Windows client</span></span>
<span data-ttu-id="1419a-145">Om du vill toouse USB-omdirigering på en dator som ansluter tooRemoteApp finns 2 åtgärder som behöver toohappen.</span><span class="sxs-lookup"><span data-stu-id="1419a-145">If you want toouse USB redirection on a computer that connects tooRemoteApp, there are 2 actions that need toohappen.</span></span> <span data-ttu-id="1419a-146">1 - administratören måste tooenable USB-omdirigering på hello samlingsnivå med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1419a-146">1 - Your administrator needs tooenable USB redirection at hello collection level by using Azure PowerShell.</span></span> <span data-ttu-id="1419a-147">2 - på varje enhet där du vill att toouse USB-omdirigering, behöver du tooenable en grupprincip som tillåter den.</span><span class="sxs-lookup"><span data-stu-id="1419a-147">2 - On each device where you want toouse USB redirection, you need tooenable a group policy that permits it.</span></span> <span data-ttu-id="1419a-148">Det här steget behöver toobe för varje användare som vill toouse USB-omdirigering.</span><span class="sxs-lookup"><span data-stu-id="1419a-148">This step will need toobe done for each user that wants toouse USB redirection.</span></span>

> [!NOTE]
> <span data-ttu-id="1419a-149">USB-omdirigering med Azure RemoteApp stöds endast för Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="1419a-149">USB redirection with Azure RemoteApp is only supported for Windows computers.</span></span>
> 
> 

### <a name="enable-usb-redirection-for-hello-remoteapp-collection"></a><span data-ttu-id="1419a-150">Aktivera USB-omdirigering för hello RemoteApp-samling</span><span class="sxs-lookup"><span data-stu-id="1419a-150">Enable USB redirection for hello RemoteApp collection</span></span>
<span data-ttu-id="1419a-151">Använd följande cmdlet tooenable USB-omdirigering på samlingsnivå hello hello:</span><span class="sxs-lookup"><span data-stu-id="1419a-151">Use hello following cmdlet tooenable USB redirection at hello collection level:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-hello-client-computer"></a><span data-ttu-id="1419a-152">Aktivera USB-omdirigering för hello klientdator</span><span class="sxs-lookup"><span data-stu-id="1419a-152">Enable USB redirection for hello client computer</span></span>
<span data-ttu-id="1419a-153">tooconfigure USB-inställningar för mappomdirigering på datorn:</span><span class="sxs-lookup"><span data-stu-id="1419a-153">tooconfigure USB redirection settings on your computer:</span></span>

1. <span data-ttu-id="1419a-154">Öppna hello Redigeraren för lokala grupprinciper (GPEDIT. MSC).</span><span class="sxs-lookup"><span data-stu-id="1419a-154">Open hello Local Group Policy Editor (GPEDIT.MSC).</span></span> <span data-ttu-id="1419a-155">(Kör gpedit.msc från en kommandotolk.)</span><span class="sxs-lookup"><span data-stu-id="1419a-155">(Run gpedit.msc from a command prompt.)</span></span>
2. <span data-ttu-id="1419a-156">Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.</span><span class="sxs-lookup"><span data-stu-id="1419a-156">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
3. <span data-ttu-id="1419a-157">Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="1419a-157">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
4. <span data-ttu-id="1419a-158">Välj **aktiverad**, och välj sedan **administratörer och användare i hello RemoteFX USB-omdirigering åtkomsträttigheter**.</span><span class="sxs-lookup"><span data-stu-id="1419a-158">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
5. <span data-ttu-id="1419a-159">Öppna en kommandotolk med administrativ behörighet och kör följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="1419a-159">Open a command prompt with administrative permissions, and run hello following command:</span></span>
   
        gpupdate /force
6. <span data-ttu-id="1419a-160">Starta om datorn hello.</span><span class="sxs-lookup"><span data-stu-id="1419a-160">Restart hello computer.</span></span>

<span data-ttu-id="1419a-161">Du kan också använda hello Grupprinciphantering verktyget toocreate och tillämpa hello USB-omdirigering av principen för alla datorer i domänen:</span><span class="sxs-lookup"><span data-stu-id="1419a-161">You can also use hello Group Policy Management tool toocreate and apply hello USB redirection policy for all computers in your domain:</span></span>

1. <span data-ttu-id="1419a-162">Logga in på hello-domänkontrollant som domänadministratör för hello.</span><span class="sxs-lookup"><span data-stu-id="1419a-162">Log into hello domain controller as hello domain administrator.</span></span>
2. <span data-ttu-id="1419a-163">Öppna hello konsolen Grupprinciphantering.</span><span class="sxs-lookup"><span data-stu-id="1419a-163">Open hello Group Policy Management Console.</span></span> <span data-ttu-id="1419a-164">(Klicka på **Start > administrativa verktyg > hantering av Grupprincip**.)</span><span class="sxs-lookup"><span data-stu-id="1419a-164">(Click **Start > Administrative Tools > Group Policy Management**.)</span></span>
3. <span data-ttu-id="1419a-165">Navigera toohello domän eller organisationsenhet som du vill toocreate hello princip.</span><span class="sxs-lookup"><span data-stu-id="1419a-165">Navigate toohello domain or organizational unit for which you want toocreate hello policy.</span></span>
4. <span data-ttu-id="1419a-166">Högerklicka på **Standarddomänprincip**, och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="1419a-166">Right-click **Default Domain Policy**, and then click **Edit**.</span></span>
5. <span data-ttu-id="1419a-167">Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.</span><span class="sxs-lookup"><span data-stu-id="1419a-167">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
6. <span data-ttu-id="1419a-168">Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="1419a-168">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
7. <span data-ttu-id="1419a-169">Välj **aktiverad**, och välj sedan **administratörer och användare i hello RemoteFX USB-omdirigering åtkomsträttigheter**.</span><span class="sxs-lookup"><span data-stu-id="1419a-169">Select **Enabled**, and then select **Administrators and Users in hello RemoteFX USB Redirection Access Rights**.</span></span>
8. <span data-ttu-id="1419a-170">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1419a-170">Click **OK**.</span></span>  

