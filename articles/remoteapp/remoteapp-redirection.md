---
title: "Med hjälp av omdirigering i Azure RemoteApp | Microsoft Docs"
description: "Lär dig hur du konfigurerar och använder omdirigering i RemoteApp"
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
ms.openlocfilehash: b5a65d129225fde46e3b090bc3cd9427989005ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="using-redirection-in-azure-remoteapp"></a><span data-ttu-id="41f1d-103">Med hjälp av omdirigering i Azure RemoteApp</span><span class="sxs-lookup"><span data-stu-id="41f1d-103">Using redirection in Azure RemoteApp</span></span>
> [!IMPORTANT]
> <span data-ttu-id="41f1d-104">Azure RemoteApp upphör att gälla den 31 augusti 2017.</span><span class="sxs-lookup"><span data-stu-id="41f1d-104">Azure RemoteApp is being discontinued on August 31, 2017.</span></span> <span data-ttu-id="41f1d-105">Läs [meddelandet](https://go.microsoft.com/fwlink/?linkid=821148) för mer information.</span><span class="sxs-lookup"><span data-stu-id="41f1d-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
> 
> 

<span data-ttu-id="41f1d-106">Omdirigering av enheter gör att användare kan interagera med fjärråtkomst appar som använder enheter som är anslutna till deras lokala datorn, telefon eller surfplatta.</span><span class="sxs-lookup"><span data-stu-id="41f1d-106">Device redirection lets your users interact with remote apps using the devices attached to their local computer, phone, or tablet.</span></span> <span data-ttu-id="41f1d-107">Om du har angett Skype via Azure RemoteApp, måste användaren kameran installerad på en dator att arbeta med Skype.</span><span class="sxs-lookup"><span data-stu-id="41f1d-107">For example, if you have provided Skype through Azure RemoteApp, your user needs the camera installed on their PC to work with Skype.</span></span> <span data-ttu-id="41f1d-108">Detta gäller även för skrivare, högtalare, Övervakare och ett intervall av USB-anslutna kringutrustning.</span><span class="sxs-lookup"><span data-stu-id="41f1d-108">This is also true for printers, speakers, monitors, and a range of USB-connected peripherals.</span></span>

<span data-ttu-id="41f1d-109">RemoteApp utnyttjar Remote Desktop Protocol (RDP) och RemoteFX för omdirigering.</span><span class="sxs-lookup"><span data-stu-id="41f1d-109">RemoteApp leverages the Remote Desktop Protocol (RDP) and RemoteFX to provide redirection.</span></span>

## <a name="what-redirection-is-enabled-by-default"></a><span data-ttu-id="41f1d-110">Vilka omdirigering är aktiverat som standard?</span><span class="sxs-lookup"><span data-stu-id="41f1d-110">What redirection is enabled by default?</span></span>
<span data-ttu-id="41f1d-111">När du använder RemoteApp är följande omdirigeringar aktiverade som standard.</span><span class="sxs-lookup"><span data-stu-id="41f1d-111">When you use RemoteApp, the following redirections are enabled by default.</span></span> <span data-ttu-id="41f1d-112">Informationen i parenteser visa RDP-inställningen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-112">The information in parentheses show the RDP setting.</span></span>

* <span data-ttu-id="41f1d-113">Spela upp ljud på den lokala datorn (**spela upp på den här datorn**).</span><span class="sxs-lookup"><span data-stu-id="41f1d-113">Play sounds on the local computer (**Play on this computer**).</span></span> <span data-ttu-id="41f1d-114">(audiomode:i:0)</span><span class="sxs-lookup"><span data-stu-id="41f1d-114">(audiomode:i:0)</span></span>
* <span data-ttu-id="41f1d-115">Fånga ljud från den lokala datorn och skickas till fjärrdatorn (**poster från den här datorn**).</span><span class="sxs-lookup"><span data-stu-id="41f1d-115">Capture audio from the local computer and send to the remote computer (**Record from this computer**).</span></span> <span data-ttu-id="41f1d-116">(audiocapturemode:i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-116">(audiocapturemode:i:1)</span></span>
* <span data-ttu-id="41f1d-117">Skriva ut till lokala skrivare (redirectprinters:i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-117">Print to local printers (redirectprinters:i:1)</span></span>
* <span data-ttu-id="41f1d-118">COM-portar (redirectcomports:i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-118">COM ports (redirectcomports:i:1)</span></span>
* <span data-ttu-id="41f1d-119">Smartkortenhet (redirectsmartcards:i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-119">Smart card device (redirectsmartcards:i:1)</span></span>
* <span data-ttu-id="41f1d-120">Urklipp (möjligheten att kopiera och klistra in) (redirectclipboard:i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-120">Clipboard (ability to copy and paste) (redirectclipboard:i:1)</span></span>
* <span data-ttu-id="41f1d-121">Avmarkera typen kantutjämning (Tillåt kantutjämning: i:1)</span><span class="sxs-lookup"><span data-stu-id="41f1d-121">Clear type font smoothing (allow font smoothing:i:1)</span></span>
* <span data-ttu-id="41f1d-122">Dirigera alla kompatibla Plug and Play-enheter.</span><span class="sxs-lookup"><span data-stu-id="41f1d-122">Redirect all supported Plug and Play devices.</span></span> <span data-ttu-id="41f1d-123">(devicestoredirect:s: *)</span><span class="sxs-lookup"><span data-stu-id="41f1d-123">(devicestoredirect:s:*)</span></span>

## <a name="what-other-redirection-is-available"></a><span data-ttu-id="41f1d-124">Vilka andra omdirigering är tillgängligt?</span><span class="sxs-lookup"><span data-stu-id="41f1d-124">What other redirection is available?</span></span>
<span data-ttu-id="41f1d-125">Två alternativ för mappomdirigering är inaktiverade som standard:</span><span class="sxs-lookup"><span data-stu-id="41f1d-125">Two redirection options are disabled by default:</span></span>

* <span data-ttu-id="41f1d-126">Omdirigering (enhetsmappning): den lokala datorn enheter bli mappade enheter i fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-126">Drive redirection (drive mapping): Your local computer's drives become mapped drives in the remote session.</span></span> <span data-ttu-id="41f1d-127">Detta kan du spara eller öppna filer från de lokala enheterna när du arbetar i fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-127">This lets you save or open files from your local drives while you work in the remote session.</span></span>
* <span data-ttu-id="41f1d-128">USB-omdirigering: du kan använda USB-enheter som är anslutna till den lokala datorn i fjärrsessionen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-128">USB redirection: You can use the USB devices attached to your local computer within the remote session.</span></span>

## <a name="change-your-redirection-settings-in-remoteapp"></a><span data-ttu-id="41f1d-129">Ändra inställningarna för omdirigering i RemoteApp</span><span class="sxs-lookup"><span data-stu-id="41f1d-129">Change your redirection settings in RemoteApp</span></span>
<span data-ttu-id="41f1d-130">Du kan ändra enhetens omdirigeringsinställningar för en samling med hjälp av Microsoft Azure PowerShell med SDK.</span><span class="sxs-lookup"><span data-stu-id="41f1d-130">You can change the device redirection settings for a collection by using the Microsoft Azure PowerShell with SDK.</span></span> <span data-ttu-id="41f1d-131">När du har installerat den nya PowerShell och SDK först konfigurera det för att hantera din prenumeration enligt beskrivningen i [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="41f1d-131">After you install the new PowerShell and SDK, first configure it to manage your subscription as described in [How to install and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="41f1d-132">Använd sedan ett kommando som liknar följande för att ange anpassade RDP-egenskaper:</span><span class="sxs-lookup"><span data-stu-id="41f1d-132">Then use a command similar to the following to set the custom RDP properties:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="41f1d-133">(Observera att  *\`n*  används som avgränsare mellan enskilda egenskaper.)</span><span class="sxs-lookup"><span data-stu-id="41f1d-133">(Note that *\`n* is used as a delimiter between individual properties.)</span></span>

<span data-ttu-id="41f1d-134">Om du vill hämta en lista över vilka anpassade RDP-egenskaper har konfigurerats, kör du följande cmdlet.</span><span class="sxs-lookup"><span data-stu-id="41f1d-134">To get a list of what custom RDP properties are configured, run the following cmdlet.</span></span> <span data-ttu-id="41f1d-135">Observera att endast anpassade egenskaper visas som utdataresultat och inte standardegenskaperna:</span><span class="sxs-lookup"><span data-stu-id="41f1d-135">Note that only custom properties are shown as output results and not the default properties:</span></span>  

    Get-AzureRemoteAppCollection -CollectionName <collection name>

<span data-ttu-id="41f1d-136">När du ställer in egenskaperna måste du ange alla anpassade egenskaper varje gång; Annars återgår inställningen till inaktiverat.</span><span class="sxs-lookup"><span data-stu-id="41f1d-136">When you set custom properties you must specify all custom properties each time; otherwise the setting reverts to disabled.</span></span>   

### <a name="common-examples"></a><span data-ttu-id="41f1d-137">Vanliga exempel</span><span class="sxs-lookup"><span data-stu-id="41f1d-137">Common examples</span></span>
<span data-ttu-id="41f1d-138">Använd följande cmdlet för att aktivera omdirigering:</span><span class="sxs-lookup"><span data-stu-id="41f1d-138">Use the following cmdlet to enable drive redirection:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*"

<span data-ttu-id="41f1d-139">Använd denna cmdlet för att aktivera både USB- och enheten omdirigering:</span><span class="sxs-lookup"><span data-stu-id="41f1d-139">Use this cmdlet to enable both USB and Drive redirection:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:*"

<span data-ttu-id="41f1d-140">Använd denna cmdlet för att inaktivera delning av Urklipp:</span><span class="sxs-lookup"><span data-stu-id="41f1d-140">Use this cmdlet to disable clipboard sharing:</span></span>  

    Set-AzureRemoteAppCollection -CollectionName <collection name>  -CustomRdpProperty "redirectclipboard:i:0"

> [!IMPORTANT]
> <span data-ttu-id="41f1d-141">Se till att helt logga ut alla användare i samlingen (och inte bara koppla från dem) innan du testar ändringen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-141">Be sure to completely log off all users in the collection (and not just disconnect them) before you test the change.</span></span> <span data-ttu-id="41f1d-142">För att säkerställa att användarna är utloggade helt, gå till den **sessioner** i samlingen i Azure-portalen och logga ut alla användare som har kopplats från eller loggat in.</span><span class="sxs-lookup"><span data-stu-id="41f1d-142">To ensure users are completely logged off, go to the **Sessions** tab in the collection in the Azure portal and log off any users who are disconnected or signed in.</span></span> <span data-ttu-id="41f1d-143">Ibland kan det ta flera sekunder för lokala enheter ska visas i Utforskaren i sessionen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-143">Sometimes it can take several seconds for the local drives to show in Explorer within the session.</span></span>
> 
> 

## <a name="change-usb-redirection-settings-on-your-windows-client"></a><span data-ttu-id="41f1d-144">Ändra inställningar för USB-omdirigering på Windows-klienter</span><span class="sxs-lookup"><span data-stu-id="41f1d-144">Change USB redirection settings on your Windows client</span></span>
<span data-ttu-id="41f1d-145">Om du vill använda USB-omdirigering på en dator som ansluter till RemoteApp finns 2 åtgärder som måste utföras.</span><span class="sxs-lookup"><span data-stu-id="41f1d-145">If you want to use USB redirection on a computer that connects to RemoteApp, there are 2 actions that need to happen.</span></span> <span data-ttu-id="41f1d-146">1 - administratören måste aktivera USB-omdirigering på samlingsnivå med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="41f1d-146">1 - Your administrator needs to enable USB redirection at the collection level by using Azure PowerShell.</span></span> <span data-ttu-id="41f1d-147">2 - du måste aktivera en grupprincip som tillåter det på varje enhet där du vill använda USB-omdirigering.</span><span class="sxs-lookup"><span data-stu-id="41f1d-147">2 - On each device where you want to use USB redirection, you need to enable a group policy that permits it.</span></span> <span data-ttu-id="41f1d-148">Det här steget måste göras för varje användare som vill använda USB-omdirigering.</span><span class="sxs-lookup"><span data-stu-id="41f1d-148">This step will need to be done for each user that wants to use USB redirection.</span></span>

> [!NOTE]
> <span data-ttu-id="41f1d-149">USB-omdirigering med Azure RemoteApp stöds endast för Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="41f1d-149">USB redirection with Azure RemoteApp is only supported for Windows computers.</span></span>
> 
> 

### <a name="enable-usb-redirection-for-the-remoteapp-collection"></a><span data-ttu-id="41f1d-150">Aktivera USB-omdirigering för RemoteApp-samling</span><span class="sxs-lookup"><span data-stu-id="41f1d-150">Enable USB redirection for the RemoteApp collection</span></span>
<span data-ttu-id="41f1d-151">Använd följande cmdlet för att aktivera USB-omdirigering på samlingsnivå:</span><span class="sxs-lookup"><span data-stu-id="41f1d-151">Use the following cmdlet to enable USB redirection at the collection level:</span></span>

    Set-AzureRemoteAppCollection -CollectionName <collection_name> -CustomRdpProperty "nusbdevicestoredirect:s:*"

### <a name="enable-usb-redirection-for-the-client-computer"></a><span data-ttu-id="41f1d-152">Aktivera USB-omdirigering för klientdatorn</span><span class="sxs-lookup"><span data-stu-id="41f1d-152">Enable USB redirection for the client computer</span></span>
<span data-ttu-id="41f1d-153">Konfigurera inställningar för USB-omdirigering på datorn:</span><span class="sxs-lookup"><span data-stu-id="41f1d-153">To configure USB redirection settings on your computer:</span></span>

1. <span data-ttu-id="41f1d-154">Öppna i Redigeraren för lokala grupprinciper (GPEDIT. MSC).</span><span class="sxs-lookup"><span data-stu-id="41f1d-154">Open the Local Group Policy Editor (GPEDIT.MSC).</span></span> <span data-ttu-id="41f1d-155">(Kör gpedit.msc från en kommandotolk.)</span><span class="sxs-lookup"><span data-stu-id="41f1d-155">(Run gpedit.msc from a command prompt.)</span></span>
2. <span data-ttu-id="41f1d-156">Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-156">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
3. <span data-ttu-id="41f1d-157">Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-157">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
4. <span data-ttu-id="41f1d-158">Välj **aktiverad**, och välj sedan **administratörer och användare i åtkomstbehörigheter för RemoteFX USB-omdirigering**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-158">Select **Enabled**, and then select **Administrators and Users in the RemoteFX USB Redirection Access Rights**.</span></span>
5. <span data-ttu-id="41f1d-159">Öppna en kommandotolk med administrativ behörighet och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="41f1d-159">Open a command prompt with administrative permissions, and run the following command:</span></span>
   
        gpupdate /force
6. <span data-ttu-id="41f1d-160">Starta om datorn.</span><span class="sxs-lookup"><span data-stu-id="41f1d-160">Restart the computer.</span></span>

<span data-ttu-id="41f1d-161">Du kan också använda verktyget hantering av Grupprincip för att skapa och använda USB-omdirigering av principen för alla datorer i domänen:</span><span class="sxs-lookup"><span data-stu-id="41f1d-161">You can also use the Group Policy Management tool to create and apply the USB redirection policy for all computers in your domain:</span></span>

1. <span data-ttu-id="41f1d-162">Logga in på domänkontrollanten som domänadministratör.</span><span class="sxs-lookup"><span data-stu-id="41f1d-162">Log into the domain controller as the domain administrator.</span></span>
2. <span data-ttu-id="41f1d-163">Öppna hanteringskonsolen för Grupprincip.</span><span class="sxs-lookup"><span data-stu-id="41f1d-163">Open the Group Policy Management Console.</span></span> <span data-ttu-id="41f1d-164">(Klicka på **Start > administrativa verktyg > hantering av Grupprincip**.)</span><span class="sxs-lookup"><span data-stu-id="41f1d-164">(Click **Start > Administrative Tools > Group Policy Management**.)</span></span>
3. <span data-ttu-id="41f1d-165">Navigera till den domän eller organisationsenhet som du vill skapa principen.</span><span class="sxs-lookup"><span data-stu-id="41f1d-165">Navigate to the domain or organizational unit for which you want to create the policy.</span></span>
4. <span data-ttu-id="41f1d-166">Högerklicka på **Standarddomänprincip**, och klicka sedan på **redigera**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-166">Right-click **Default Domain Policy**, and then click **Edit**.</span></span>
5. <span data-ttu-id="41f1d-167">Öppna **datorn Datorkonfiguration\Principer\Administrativa mallar\Windows-komponenter\Fjärrskrivbordstjänster\Värdserver för anslutning till fjärrskrivbord Client\RemoteFX USB-enhetsomdirigering**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-167">Open **Computer Configuration\Policies\Administrative Templates\Windows Components\Remote Desktop Services\Remote Desktop Connection Client\RemoteFX USB Device Redirection**.</span></span>
6. <span data-ttu-id="41f1d-168">Dubbelklicka på **Tillåt RDP-omdirigering av andra stöds RemoteFX USB-enheter från den här datorn**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-168">Double-click **Allow RDP redirection of other supported RemoteFX USB devices from this computer**.</span></span>
7. <span data-ttu-id="41f1d-169">Välj **aktiverad**, och välj sedan **administratörer och användare i åtkomstbehörigheter för RemoteFX USB-omdirigering**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-169">Select **Enabled**, and then select **Administrators and Users in the RemoteFX USB Redirection Access Rights**.</span></span>
8. <span data-ttu-id="41f1d-170">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="41f1d-170">Click **OK**.</span></span>  

