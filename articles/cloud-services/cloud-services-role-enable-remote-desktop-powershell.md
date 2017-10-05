---
title: "Aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services med hjälp av PowerShell"
description: "Hur du konfigurerar ditt tjänstprogram för azure-molnet som använder PowerShell för att tillåta anslutningar till fjärrskrivbord"
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
ms.openlocfilehash: 171f27c92ee9de14301ebb664e9ba3bcd98c394d
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a><span data-ttu-id="5cd50-103">Aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="5cd50-103">Enable Remote Desktop Connection for a Role in Azure Cloud Services using PowerShell</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="5cd50-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="5cd50-104">Azure portal</span></span>](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [<span data-ttu-id="5cd50-105">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="5cd50-105">Azure classic portal</span></span>](cloud-services-role-enable-remote-desktop.md)
> * [<span data-ttu-id="5cd50-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="5cd50-106">PowerShell</span></span>](cloud-services-role-enable-remote-desktop-powershell.md)
> * [<span data-ttu-id="5cd50-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5cd50-107">Visual Studio</span></span>](../vs-azure-tools-remote-desktop-roles.md)
>
>

<span data-ttu-id="5cd50-108">Fjärrskrivbord kan du få åtkomst till skrivbordet på en roll som körs i Azure.</span><span class="sxs-lookup"><span data-stu-id="5cd50-108">Remote Desktop enables you to access the desktop of a role running in Azure.</span></span> <span data-ttu-id="5cd50-109">Du kan använda anslutning till fjärrskrivbord för att felsöka och diagnostisera problem med programmet när den körs.</span><span class="sxs-lookup"><span data-stu-id="5cd50-109">You can use a Remote Desktop connection to troubleshoot and diagnose problems with your application while it is running.</span></span>

<span data-ttu-id="5cd50-110">Den här artikeln beskriver hur du aktiverar fjärrskrivbord på dina Molntjänstroller med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="5cd50-110">This article describes how to enable remote desktop on your Cloud Service Roles using PowerShell.</span></span> <span data-ttu-id="5cd50-111">Se [hur du installerar och konfigurerar du Azure PowerShell](/powershell/azure/overview) för kraven för den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="5cd50-111">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for the prerequisites needed for this article.</span></span> <span data-ttu-id="5cd50-112">PowerShell använder Remote Desktop-tillägget så att du kan aktivera Fjärrskrivbord när programmet distribueras.</span><span class="sxs-lookup"><span data-stu-id="5cd50-112">PowerShell utilizes the Remote Desktop Extension so you can enable Remote Desktop after the application is deployed.</span></span>

## <a name="configure-remote-desktop-from-powershell"></a><span data-ttu-id="5cd50-113">Konfigurera fjärrskrivbord från PowerShell</span><span class="sxs-lookup"><span data-stu-id="5cd50-113">Configure Remote Desktop from PowerShell</span></span>
<span data-ttu-id="5cd50-114">Den [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet kan du aktivera Fjärrskrivbord på angivna roller eller alla roller i distributionen cloud service.</span><span class="sxs-lookup"><span data-stu-id="5cd50-114">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet allows you to enable Remote Desktop on specified roles or all roles of your cloud service deployment.</span></span> <span data-ttu-id="5cd50-115">Cmdleten kan du ange användarnamnet och lösenordet för användaren av fjärrskrivbord via den *autentiseringsuppgifter* parameter som accepterar ett PSCredential-objekt.</span><span class="sxs-lookup"><span data-stu-id="5cd50-115">The cmdlet lets you specify the Username and Password for the remote desktop user through the *Credential* parameter that accepts a PSCredential object.</span></span>

<span data-ttu-id="5cd50-116">Om du använder PowerShell interaktivt kan du enkelt skapa PSCredential-objekt genom att anropa den [Get-autentiseringsuppgifter](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5cd50-116">If you are using PowerShell interactively, you can easily set the PSCredential object by calling the [Get-Credentials](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.</span></span>

```
$remoteusercredentials = Get-Credential
```

<span data-ttu-id="5cd50-117">Detta kommando visar en dialogruta där du kan ange användarnamn och lösenord för användaren på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="5cd50-117">This command displays a dialog box allowing you to enter the username and password for the remote user in a secure manner.</span></span>

<span data-ttu-id="5cd50-118">Eftersom PowerShell hjälper i automatiseringsscenarier, kan du också ställa in den **PSCredential** objekt på ett sätt som inte kräver användaråtgärder.</span><span class="sxs-lookup"><span data-stu-id="5cd50-118">Since PowerShell helps in automation scenarios, you can also set up the **PSCredential** object in a way that doesn't require user interaction.</span></span> <span data-ttu-id="5cd50-119">Du måste först skapa ett säkert lösenord.</span><span class="sxs-lookup"><span data-stu-id="5cd50-119">First, you need to set up a secure password.</span></span> <span data-ttu-id="5cd50-120">Du börjar med att ange ett lösenord i oformaterad text konvertera den till en säker sträng med hjälp av [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd50-120">You begin with specifying a plain text password convert it to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span> <span data-ttu-id="5cd50-121">Därefter måste du konvertera den här säker sträng till en krypterad sträng som standard med hjälp av [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd50-121">Next you need to convert this secure string into an encrypted standard string using [ConvertFrom-SecureString](https://technet.microsoft.com/library/hh849814.aspx).</span></span> <span data-ttu-id="5cd50-122">Nu kan du spara den här krypterad sträng som standard till en fil med hjälp av [Set-innehåll](https://technet.microsoft.com/library/ee176959.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd50-122">Now you can save this encrypted standard string to a file using [Set-Content](https://technet.microsoft.com/library/ee176959.aspx).</span></span>

<span data-ttu-id="5cd50-123">Du kan också skapa en fil säkert lösenord så att du inte behöver skriva in lösenordet varje gång.</span><span class="sxs-lookup"><span data-stu-id="5cd50-123">You can also create a secure password file so that you don't have to type in the password every time.</span></span> <span data-ttu-id="5cd50-124">En fil säkert lösenord är också bättre än oformaterad text.</span><span class="sxs-lookup"><span data-stu-id="5cd50-124">Also, a secure password file is better than a plain text file.</span></span> <span data-ttu-id="5cd50-125">Använd följande PowerShell för att skapa en fil säkert lösenord:</span><span class="sxs-lookup"><span data-stu-id="5cd50-125">Use the following PowerShell to create a secure password file:</span></span>

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> <span data-ttu-id="5cd50-126">När du ställer in lösenordet kontrollerar du att du uppfyller de [krav på komplexitet](https://technet.microsoft.com/library/cc786468.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd50-126">When setting the password, make sure that you meet the [complexity requirements](https://technet.microsoft.com/library/cc786468.aspx).</span></span>
>
>

<span data-ttu-id="5cd50-127">För att skapa autentiseringsobjektet från filen säkert lösenord, måste du läsa innehållet i filen och omvandla dem till en säker sträng med hjälp av [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span><span class="sxs-lookup"><span data-stu-id="5cd50-127">To create the credential object from the secure password file, you must read the file contents and convert them back to a secure string using [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).</span></span>

<span data-ttu-id="5cd50-128">Den [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet accepterar också en *giltighetstid* som anger en **DateTime** vid som användarkontot upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="5cd50-128">The [Set-AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet also accepts an *Expiration* parameter, which specifies a **DateTime** at which the user account expires.</span></span> <span data-ttu-id="5cd50-129">Du kan till exempel ange kontot som ska gälla några dagar från det aktuella datum och tid.</span><span class="sxs-lookup"><span data-stu-id="5cd50-129">For example, you could set the account to expire a few days from the current date and time.</span></span>

<span data-ttu-id="5cd50-130">Det här PowerShell-exemplet visar hur du ställer Remote Desktop-tillägget på en tjänst i molnet:</span><span class="sxs-lookup"><span data-stu-id="5cd50-130">This PowerShell example shows you how to set the Remote Desktop Extension on a cloud service:</span></span>

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
<span data-ttu-id="5cd50-131">Du kan också ange distributionsplatsen och roller som du vill aktivera Fjärrskrivbord på.</span><span class="sxs-lookup"><span data-stu-id="5cd50-131">You can also optionally specify the deployment slot and roles that you want to enable remote desktop on.</span></span> <span data-ttu-id="5cd50-132">Om dessa parametrar anges, cmdlet aktiverar fjärrskrivbord på alla roller i den **produktion** distributionsplatsen.</span><span class="sxs-lookup"><span data-stu-id="5cd50-132">If these parameters are not specified, the cmdlet enables remote desktop on all roles in the **Production** deployment slot.</span></span>

<span data-ttu-id="5cd50-133">Tillägget för fjärrskrivbord är associerade med en distribution.</span><span class="sxs-lookup"><span data-stu-id="5cd50-133">The Remote Desktop extension is associated with a deployment.</span></span> <span data-ttu-id="5cd50-134">Om du skapar en ny distribution för tjänsten som du behöver aktivera Fjärrskrivbord på distributionen.</span><span class="sxs-lookup"><span data-stu-id="5cd50-134">If you create a new deployment for the service, you have to enable remote desktop on that deployment.</span></span> <span data-ttu-id="5cd50-135">Om du alltid vill ha fjärrskrivbord aktiverat bör du överväga integrering av PowerShell-skript i ditt arbetsflöde för distribution.</span><span class="sxs-lookup"><span data-stu-id="5cd50-135">If you always want to have remote desktop enabled, then you should consider integrating the PowerShell scripts into your deployment workflow.</span></span>

## <a name="remote-desktop-into-a-role-instance"></a><span data-ttu-id="5cd50-136">Fjärrskrivbord till en rollinstans</span><span class="sxs-lookup"><span data-stu-id="5cd50-136">Remote Desktop into a role instance</span></span>
<span data-ttu-id="5cd50-137">Den [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet är används för att remote desktop i en viss rollinstans av Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="5cd50-137">The [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet is used to remote desktop into a specific role instance of your cloud service.</span></span> <span data-ttu-id="5cd50-138">Du kan använda den *LocalPath* parametern för att ladda ned RDP-filen lokalt.</span><span class="sxs-lookup"><span data-stu-id="5cd50-138">You can use the *LocalPath* parameter to download the RDP file locally.</span></span> <span data-ttu-id="5cd50-139">Eller så kan du använda den *starta* parametern för att starta dialogrutan anslutning till fjärrskrivbord för att få åtkomst till molnet rollen tjänstinstansen direkt.</span><span class="sxs-lookup"><span data-stu-id="5cd50-139">Or you can use the *Launch* parameter to directly launch the Remote Desktop Connection dialog to access the cloud service role instance.</span></span>

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a><span data-ttu-id="5cd50-140">Kontrollera om tillägget för fjärrskrivbord är aktiverat på en tjänst</span><span class="sxs-lookup"><span data-stu-id="5cd50-140">Check if Remote Desktop extension is enabled on a service</span></span>
<span data-ttu-id="5cd50-141">Den [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdleten visas som fjärrskrivbord är aktiverat eller inaktiverat på en tjänstdistribution.</span><span class="sxs-lookup"><span data-stu-id="5cd50-141">The [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet displays that remote desktop is enabled or disabled on a service deployment.</span></span> <span data-ttu-id="5cd50-142">Cmdleten returnerar användarnamnet för användaren av fjärrskrivbord och de roller som remote desktop tillägget har aktiverats för.</span><span class="sxs-lookup"><span data-stu-id="5cd50-142">The cmdlet returns the username for the remote desktop user and the roles that the remote desktop extension is enabled for.</span></span> <span data-ttu-id="5cd50-143">Som standard det som händer på distributionsplatsen och du kan välja att använda mellanlagringsplatsen i stället.</span><span class="sxs-lookup"><span data-stu-id="5cd50-143">By default, this happens on the deployment slot and you can choose to use the staging slot instead.</span></span>

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a><span data-ttu-id="5cd50-144">Ta bort tillägget för fjärrskrivbord från en tjänst</span><span class="sxs-lookup"><span data-stu-id="5cd50-144">Remove Remote Desktop extension from a service</span></span>
<span data-ttu-id="5cd50-145">Om du redan har aktiverat remote desktop tillägg på en distribution och uppdatera inställningarna för fjärrskrivbordet, först ta bort tillägget.</span><span class="sxs-lookup"><span data-stu-id="5cd50-145">If you have already enabled the remote desktop extension on a deployment, and need to update the remote desktop settings, first remove the extension.</span></span> <span data-ttu-id="5cd50-146">Och aktivera det igen med de nya inställningarna.</span><span class="sxs-lookup"><span data-stu-id="5cd50-146">And enable it again with the new settings.</span></span> <span data-ttu-id="5cd50-147">Till exempel om du vill ange ett nytt lösenord för användarkontot eller kontot har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="5cd50-147">For example, if you want to set a new password for the remote user account, or the account expired.</span></span> <span data-ttu-id="5cd50-148">Detta krävs på befintliga distributioner som har filnamnstillägget remote desktop aktiverad.</span><span class="sxs-lookup"><span data-stu-id="5cd50-148">Doing this is required on existing deployments that have the remote desktop extension enabled.</span></span> <span data-ttu-id="5cd50-149">För nya distributioner kan du bara använda tillägget direkt.</span><span class="sxs-lookup"><span data-stu-id="5cd50-149">For new deployments, you can simply apply the extension directly.</span></span>

<span data-ttu-id="5cd50-150">Du kan använda för att ta bort tillägget för remote desktop från distributionen av [ta bort AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="5cd50-150">To remove the remote desktop extension from the deployment, you can use the [Remove-AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet.</span></span> <span data-ttu-id="5cd50-151">Du kan också ange distributionsplatsen och roll som du vill ta bort tillägget för remote desktop.</span><span class="sxs-lookup"><span data-stu-id="5cd50-151">You can also optionally specify the deployment slot and role from which you want to remove the remote desktop extension.</span></span>

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> <span data-ttu-id="5cd50-152">Om du vill ta bort tilläggets konfiguration, bör du anropar den *ta bort* med den **UninstallConfiguration** parameter.</span><span class="sxs-lookup"><span data-stu-id="5cd50-152">To completely remove the extension configuration, you should call the *remove* cmdlet with the **UninstallConfiguration** parameter.</span></span>
>
> <span data-ttu-id="5cd50-153">Den **UninstallConfiguration** parametern avinstallerar tillägget konfiguration som tillämpas på tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5cd50-153">The **UninstallConfiguration** parameter uninstalls any extension configuration that is applied to the service.</span></span> <span data-ttu-id="5cd50-154">Konfiguration för varje är associerad med konfigurationen av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5cd50-154">Every extension configuration is associated with the service configuration.</span></span> <span data-ttu-id="5cd50-155">Anropar den *ta bort* cmdlet utan **UninstallConfiguration** tas den <mark>distribution</mark> tilläggets konfiguration, vilket effektivt tar bort tillägget.</span><span class="sxs-lookup"><span data-stu-id="5cd50-155">Calling the *remove* cmdlet without **UninstallConfiguration** disassociates the <mark>deployment</mark> from the extension configuration, thus effectively removing the extension.</span></span> <span data-ttu-id="5cd50-156">Tilläggets konfiguration förblir dock kopplat till tjänsten.</span><span class="sxs-lookup"><span data-stu-id="5cd50-156">However, the extension configuration remains associated with the service.</span></span>
>
>

## <a name="additional-resources"></a><span data-ttu-id="5cd50-157">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="5cd50-157">Additional resources</span></span>

<span data-ttu-id="5cd50-158">[Hur du konfigurerar molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)</span><span class="sxs-lookup"><span data-stu-id="5cd50-158">[How to Configure Cloud Services](cloud-services-how-to-configure.md)
[Cloud services FAQ - Remote Desktop](cloud-services-faq.md)</span></span>
