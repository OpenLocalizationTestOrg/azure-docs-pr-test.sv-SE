---
title: "aaaEnable anslutning till fjärrskrivbord för en roll i Azure Cloud Services med hjälp av PowerShell"
description: "Hur tooconfigure din azure cloud service-program med hjälp av PowerShell tooallow anslutningar till fjärrskrivbord"
services: cloud-services
documentationcenter: 
author: thraka
manager: timlt
editor: 
ms.assetid: bf2f70a4-20dc-4302-a91a-38cd7a2baa62
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: adegeo
ms.openlocfilehash: 3f46b014f29f1c0be0e1b485d2f0152424162bb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="787c0-103">Aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="787c0-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="787c0-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="787c0-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="787c0-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="787c0-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="787c0-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="787c0-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="787c0-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="787c0-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="787c0-108">Fjärrskrivbord kan du tooaccess hello skrivbordet för en roll som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="787c0-108">Remote Desktop enables you tooaccess hello desktop of a role running in Azure.</span></span> <span data-ttu-id="787c0-109">Du kan använda en Fjärrskrivbord anslutning tootroubleshoot och diagnostisera problem med ditt program när den körs.</span><span class="sxs-lookup"><span data-stu-id="787c0-109">You can use a Remote Desktop connection tootroubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="787c0-110">Den här artikeln beskriver hur tooenable fjärrskrivbord på dina Molntjänstroller med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="787c0-110">This article describes how tooenable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="787c0-111">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="787c0-111">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for hello prerequisites needed for this article.</span></span> <span data-ttu-id="787c0-112">PowerShell använder hello Remote Desktop-tillägget så att du kan aktivera Fjärrskrivbord när hello programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="787c0-112">PowerShell utilizes hello Remote Desktop Extension so you can enable Remote Desktop after hello application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="787c0-113">Konfigurera fjärrskrivbord från PowerShell</span><span class="sxs-lookup"><span data-stu-id="787c0-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="787c0-114">Hej [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) kan du använda cmdleten tooenable fjärrskrivbord på angivna roller eller alla roller i distributionen cloud service.</span><span class="sxs-lookup"><span data-stu-id="787c0-114">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you tooenable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="787c0-115">hello-cmdleten låter dig ange hello användarnamn och lösenord för hello användare av fjärrskrivbord via hello *autentiseringsuppgifter* parameter som accepterar ett PSCredential-objekt.</span><span class="sxs-lookup"><span data-stu-id="787c0-115">hello cmdlet lets you specify hello Username and Password for hello remote desktop user through hello *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="787c0-116">Om du använder PowerShell interaktivt kan du enkelt skapa hello PSCredential-objekt genom att anropa hello [Get-autentiseringsuppgifter](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="787c0-116">If you are using PowerShell interactively, you can easily set hello PSCredential object by calling hello [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="787c0-117">Detta kommando visar en dialogruta där du tooenter hello användarnamn och lösenord för hello fjärranslutna användare på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="787c0-117">This command displays a dialog box allowing you tooenter hello username and password for hello remote user in a secure manner.</span></span>

<span data-ttu-id="787c0-118">Eftersom PowerShell hjälper i automatiseringsscenarier, kan du också ställa in hello **PSCredential** objekt på ett sätt som inte kräver användaråtgärder.</span><span class="sxs-lookup"><span data-stu-id="787c0-118">Since PowerShell helps in automation scenarios, you can also set up hello **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="787c0-119">Du måste först tooset på ett säkert lösenord.</span><span class="sxs-lookup"><span data-stu-id="787c0-119">First, you need tooset up a secure password.</span></span> <span data-ttu-id="787c0-120">Du börjar med att ange ett lösenord i oformaterad text konvertera den tooa säker sträng med [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="787c0-120">You begin with specifying a plain text password convert it tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="787c0-121">Därefter behöver du tooconvert säker strängen till en krypterad sträng som standard med hjälp av [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="787c0-121">Next you need tooconvert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="787c0-122">Nu kan du spara den här krypterade standard tooa filen med hjälp av [Set-innehåll](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="787c0-122">Now you can save this encrypted standard string tooa file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="787c0-123">Du kan också skapa en fil säkert lösenord så att du inte har tootype i hello lösenord varje gång.</span><span class="sxs-lookup"><span data-stu-id="787c0-123">You can also create a secure password file so that you don't have tootype in hello password every time.</span></span> <span data-ttu-id="787c0-124">En fil säkert lösenord är också bättre än oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="787c0-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="787c0-125">Använd följande PowerShell-toocreate en fil säkert lösenord hello:</span><span class="sxs-lookup"><span data-stu-id="787c0-125">Use hello following PowerShell toocreate a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="787c0-126">När du ställer in hello lösenord, kontrollerar du att du uppfyller hello [krav på komplexitet](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="787c0-126">When setting hello password, make sure that you meet hello [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="787c0-127">toocreate hello-autentiseringsobjekt från hello säkert lösenordsfilen Läs hello filinnehållet och konvertera dem tillbaka tooa säker med hjälp av [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="787c0-127">toocreate hello credential object from hello secure password file, you must read hello file contents and convert them back tooa secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="787c0-128">Hej [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet accepterar också en *upphör att gälla* som anger en **DateTime** på vilken hello användare kontot upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="787c0-128">hello [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which hello user account expires.</span></span> <span data-ttu-id="787c0-129">Du kan till exempel ange hello konto tooexpire några dagar från hello aktuellt datum och tid.</span><span class="sxs-lookup"><span data-stu-id="787c0-129">For example, you could set hello account tooexpire a few days from hello current date and time.</span></span>

<span data-ttu-id="787c0-130">Det här PowerShell-exemplet visar hur tooset hello Remote Desktop-tillägg på en tjänst i molnet:</span><span class="sxs-lookup"><span data-stu-id="787c0-130">This PowerShell example shows you how tooset hello Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="787c0-131">Du kan också ange hello distributionsplatsen och roller som du vill använda tooenable fjärrskrivbord på.</span><span class="sxs-lookup"><span data-stu-id="787c0-131">You can also optionally specify hello deployment slot and roles that you want tooenable remote desktop on.</span></span> <span data-ttu-id="787c0-132">Om dessa parametrar anges, hello kan fjärrskrivbord på alla roller i hello **produktion** distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="787c0-132">If these parameters are not specified, hello cmdlet enables remote desktop on all roles in hello **Production** deployment slot.</span></span>

<span data-ttu-id="787c0-133">hello fjärrskrivbord tillägget är associerad med en distribution.</span><span class="sxs-lookup"><span data-stu-id="787c0-133">hello Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="787c0-134">Om du skapar en ny distribution för hello-tjänsten kan ha tooenable fjärrskrivbord på distributionen.</span><span class="sxs-lookup"><span data-stu-id="787c0-134">If you create a new deployment for hello service, you have tooenable remote desktop on that deployment.</span></span> <span data-ttu-id="787c0-135">Om du alltid vill toohave fjärrskrivbord aktiverat, bör du överväga integrera hello PowerShell-skript i ditt arbetsflöde för distribution.</span><span class="sxs-lookup"><span data-stu-id="787c0-135">If you always want toohave remote desktop enabled, then you should consider integrating hello PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="787c0-136">Fjärrskrivbord till en rollinstans</span><span class="sxs-lookup"><span data-stu-id="787c0-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="787c0-137">Hej [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet har använt tooremote skrivbordet till en viss rollinstans av Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="787c0-137">hello [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used tooremote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="787c0-138">Du kan använda hello *LocalPath* parametern toodownload hello RDP-filen lokalt.</span><span class="sxs-lookup"><span data-stu-id="787c0-138">You can use hello *LocalPath* parameter toodownload hello RDP file locally.</span></span> <span data-ttu-id="787c0-139">Du kan också använda hello *starta* parametern toodirectly starta hello anslutning till fjärrskrivbord dialogrutan tooaccess hello molnet tjänstinstans för rollen.</span><span class="sxs-lookup"><span data-stu-id="787c0-139">Or you can use hello *Launch* parameter toodirectly launch hello Remote Desktop Connection dialog tooaccess hello cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="787c0-140">Kontrollera om tillägget för fjärrskrivbord är aktiverat på en tjänst</span><span class="sxs-lookup"><span data-stu-id="787c0-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="787c0-141">Hej [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdleten visas som fjärrskrivbord är aktiverat eller inaktiverat på en tjänstdistribution.</span><span class="sxs-lookup"><span data-stu-id="787c0-141">hello [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="787c0-142">hello cmdlet returnerar hello användarnamn för hello fjärrskrivbordsanvändare och hello roller som hello remote desktop tillägget har aktiverats för.</span><span class="sxs-lookup"><span data-stu-id="787c0-142">hello cmdlet returns hello username for hello remote desktop user and hello roles that hello remote desktop extension is enabled for.</span></span> <span data-ttu-id="787c0-143">Detta inträffar hello distributionsplatsen som standard och du kan välja toouse hello mellanlagringsplatsen i stället.</span><span class="sxs-lookup"><span data-stu-id="787c0-143">By default, this happens on hello deployment slot and you can choose toouse hello staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="787c0-144">Ta bort tillägget för fjärrskrivbord från en tjänst</span><span class="sxs-lookup"><span data-stu-id="787c0-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="787c0-145">Om du redan har aktiverat hello remote desktop tillägg på en distribution och behöver tooupdate hello inställningarna för fjärrskrivbordet, först ta bort hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="787c0-145">If you have already enabled hello remote desktop extension on a deployment, and need tooupdate hello remote desktop settings, first remove hello extension.</span></span> <span data-ttu-id="787c0-146">Och aktivera det igen med hello nya inställningar.</span><span class="sxs-lookup"><span data-stu-id="787c0-146">And enable it again with hello new settings.</span></span> <span data-ttu-id="787c0-147">Till exempel om du vill tooset ett nytt lösenord för hello fjärranvändare konto eller hello upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="787c0-147">For example, if you want tooset a new password for hello remote user account, or hello account expired.</span></span> <span data-ttu-id="787c0-148">Detta krävs på befintliga distributioner som har hello remote desktop tillägget aktiverat.</span><span class="sxs-lookup"><span data-stu-id="787c0-148">Doing this is required on existing deployments that have hello remote desktop extension enabled.</span></span> <span data-ttu-id="787c0-149">För nya distributioner kan du bara använda hello tillägget direkt.</span><span class="sxs-lookup"><span data-stu-id="787c0-149">For new deployments, you can simply apply hello extension directly.</span></span>

<span data-ttu-id="787c0-150">tooremove hello remote desktop-tillägg från hello distribution, kan du använda hello [ta bort AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="787c0-150">tooremove hello remote desktop extension from hello deployment, you can use hello [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="787c0-151">Du kan också ange hello distributionsplatsen och roll som du vill tooremove hello remote desktop tillägg.</span><span class="sxs-lookup"><span data-stu-id="787c0-151">You can also optionally specify hello deployment slot and role from which you want tooremove hello remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="787c0-152">toocompletely ta bort hello tilläggets konfiguration ska du anropa hello *ta bort* med hello **UninstallConfiguration** parameter.</span><span class="sxs-lookup"><span data-stu-id="787c0-152">toocompletely remove hello extension configuration, you should call hello *remove* cmdlet with hello **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="787c0-153">Hej **UninstallConfiguration** parametern avinstallerar konfiguration för alla som är kopplade toohello service.</span><span class="sxs-lookup"><span data-stu-id="787c0-153">hello **UninstallConfiguration** parameter uninstalls any extension configuration that is applied toohello service.</span></span> <span data-ttu-id="787c0-154">Konfiguration för varje är associerad med hello tjänstkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="787c0-154">Every extension configuration is associated with hello service configuration.</span></span> <span data-ttu-id="787c0-155">Anropar hello *ta bort* cmdlet utan **UninstallConfiguration** tar bort associationen hello <mark>distribution</mark> hello konfiguration, vilket effektivt tar bort hello-tillägget.</span><span class="sxs-lookup"><span data-stu-id="787c0-155">Calling hello *remove* cmdlet without **UninstallConfiguration** disassociates hello <mark>deployment</mark> from hello extension configuration, thus effectively removing hello extension.</span></span> <span data-ttu-id="787c0-156">Hello tillägget konfigurationen förblir dock associerad med hello-tjänst.</span><span class="sxs-lookup"><span data-stu-id="787c0-156">However, hello extension configuration remains associated with hello service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="787c0-157">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="787c0-157">Additional resources</span></span>

<span data-ttu-id="787c0-158">[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="787c0-158">[How tooConfigure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
