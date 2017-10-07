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
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services-using-powershell"></a>Aktivera anslutning till fjärrskrivbord för en roll i Azure Cloud Services med hjälp av PowerShell
> [!div class="op_single_selector"]
> * [Azure Portal](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Klassisk Azure-portal](cloud-services-role-enable-remote-desktop.md)
> * [PowerShell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](../vs-azure-tools-remote-desktop-roles.md)
>
>

Fjärrskrivbord kan du tooaccess hello skrivbordet för en roll som körs i Azure. Du kan använda en Fjärrskrivbord anslutning tootroubleshoot och diagnostisera problem med ditt program när den körs.

Den här artikeln beskriver hur tooenable fjärrskrivbord på dina Molntjänstroller med hjälp av PowerShell. Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för hello kraven för den här artikeln. PowerShell använder hello Remote Desktop-tillägget så att du kan aktivera Fjärrskrivbord när hello programmet distribueras.

## <a name="configure-remote-desktop-from-powershell"></a>Konfigurera fjärrskrivbord från PowerShell
Hej [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) kan du använda cmdleten tooenable fjärrskrivbord på angivna roller eller alla roller i distributionen cloud service. hello-cmdleten låter dig ange hello användarnamn och lösenord för hello användare av fjärrskrivbord via hello *autentiseringsuppgifter* parameter som accepterar ett PSCredential-objekt.

Om du använder PowerShell interaktivt kan du enkelt skapa hello PSCredential-objekt genom att anropa hello [Get-autentiseringsuppgifter](https://technet.microsoft.com/library/hh849815.aspx) cmdlet.

```
$remoteusercredentials = Get-Credential
```

Detta kommando visar en dialogruta där du tooenter hello användarnamn och lösenord för hello fjärranslutna användare på ett säkert sätt.

Eftersom PowerShell hjälper i automatiseringsscenarier, kan du också ställa in hello **PSCredential** objekt på ett sätt som inte kräver användaråtgärder. Du måste först tooset på ett säkert lösenord. Du börjar med att ange ett lösenord i oformaterad text konvertera den tooa säker sträng med [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx). Därefter behöver du tooconvert säker strängen till en krypterad sträng som standard med hjälp av [ConvertFrom SecureString](https://technet.microsoft.com/library/hh849814.aspx). Nu kan du spara den här krypterade standard tooa filen med hjälp av [Set-innehåll](https://technet.microsoft.com/library/ee176959.aspx).

Du kan också skapa en fil säkert lösenord så att du inte har tootype i hello lösenord varje gång. En fil säkert lösenord är också bättre än oformaterad text. Använd följande PowerShell-toocreate en fil säkert lösenord hello:

```
ConvertTo-SecureString -String "Password123" -AsPlainText -Force | ConvertFrom-SecureString | Set-Content "password.txt"
```

> [!IMPORTANT]
> När du ställer in hello lösenord, kontrollerar du att du uppfyller hello [krav på komplexitet](https://technet.microsoft.com/library/cc786468.aspx).
>
>

toocreate hello-autentiseringsobjekt från hello säkert lösenordsfilen Läs hello filinnehållet och konvertera dem tillbaka tooa säker med hjälp av [ConvertTo-SecureString](https://technet.microsoft.com/library/hh849818.aspx).

Hej [Set AzureServiceRemoteDesktopExtension](/powershell/module/azure/set-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet accepterar också en *upphör att gälla* som anger en **DateTime** på vilken hello användare kontot upphör att gälla. Du kan till exempel ange hello konto tooexpire några dagar från hello aktuellt datum och tid.

Det här PowerShell-exemplet visar hur tooset hello Remote Desktop-tillägg på en tjänst i molnet:

```
$servicename = "cloudservice"
$username = "RemoteDesktopUser"
$securepassword = Get-Content -Path "password.txt" | ConvertTo-SecureString
$expiry = $(Get-Date).AddDays(1)
$credential = New-Object System.Management.Automation.PSCredential $username,$securepassword
Set-AzureServiceRemoteDesktopExtension -ServiceName $servicename -Credential $credential -Expiration $expiry
```
Du kan också ange hello distributionsplatsen och roller som du vill använda tooenable fjärrskrivbord på. Om dessa parametrar anges, hello kan fjärrskrivbord på alla roller i hello **produktion** distributionsplatsen.

hello fjärrskrivbord tillägget är associerad med en distribution. Om du skapar en ny distribution för hello-tjänsten kan ha tooenable fjärrskrivbord på distributionen. Om du alltid vill toohave fjärrskrivbord aktiverat, bör du överväga integrera hello PowerShell-skript i ditt arbetsflöde för distribution.

## <a name="remote-desktop-into-a-role-instance"></a>Fjärrskrivbord till en rollinstans
Hej [Get-AzureRemoteDesktopFile](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdlet har använt tooremote skrivbordet till en viss rollinstans av Molntjänsten. Du kan använda hello *LocalPath* parametern toodownload hello RDP-filen lokalt. Du kan också använda hello *starta* parametern toodirectly starta hello anslutning till fjärrskrivbord dialogrutan tooaccess hello molnet tjänstinstans för rollen.

```
Get-AzureRemoteDesktopFile -ServiceName $servicename -Name "WorkerRole1_IN_0" -Launch
```


## <a name="check-if-remote-desktop-extension-is-enabled-on-a-service"></a>Kontrollera om tillägget för fjärrskrivbord är aktiverat på en tjänst
Hej [Get-AzureServiceRemoteDesktopExtension](/powershell/module/azure/get-azureremotedesktopfile?view=azuresmps-3.7.0) cmdleten visas som fjärrskrivbord är aktiverat eller inaktiverat på en tjänstdistribution. hello cmdlet returnerar hello användarnamn för hello fjärrskrivbordsanvändare och hello roller som hello remote desktop tillägget har aktiverats för. Detta inträffar hello distributionsplatsen som standard och du kan välja toouse hello mellanlagringsplatsen i stället.

```
Get-AzureServiceRemoteDesktopExtension -ServiceName $servicename
```

## <a name="remove-remote-desktop-extension-from-a-service"></a>Ta bort tillägget för fjärrskrivbord från en tjänst
Om du redan har aktiverat hello remote desktop tillägg på en distribution och behöver tooupdate hello inställningarna för fjärrskrivbordet, först ta bort hello-tillägget. Och aktivera det igen med hello nya inställningar. Till exempel om du vill tooset ett nytt lösenord för hello fjärranvändare konto eller hello upphört att gälla. Detta krävs på befintliga distributioner som har hello remote desktop tillägget aktiverat. För nya distributioner kan du bara använda hello tillägget direkt.

tooremove hello remote desktop-tillägg från hello distribution, kan du använda hello [ta bort AzureServiceRemoteDesktopExtension](/powershell/module/azure/remove-azureserviceremotedesktopextension?view=azuresmps-3.7.0) cmdlet. Du kan också ange hello distributionsplatsen och roll som du vill tooremove hello remote desktop tillägg.

```
Remove-AzureServiceRemoteDesktopExtension -ServiceName $servicename -UninstallConfiguration
```

> [!NOTE]
> toocompletely ta bort hello tilläggets konfiguration ska du anropa hello *ta bort* med hello **UninstallConfiguration** parameter.
>
> Hej **UninstallConfiguration** parametern avinstallerar konfiguration för alla som är kopplade toohello service. Konfiguration för varje är associerad med hello tjänstkonfigurationen. Anropar hello *ta bort* cmdlet utan **UninstallConfiguration** tar bort associationen hello <mark>distribution</mark> hello konfiguration, vilket effektivt tar bort hello-tillägget. Hello tillägget konfigurationen förblir dock associerad med hello-tjänst.
>
>

## <a name="additional-resources"></a>Ytterligare resurser

[Hur tooConfigure molntjänster](cloud-services-how-to-configure.md)
[Cloud services vanliga frågor och svar - fjärrskrivbord](cloud-services-faq.md)
