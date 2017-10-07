---
title: aaaUse PowerShell-cmdlets med Azure RemoteApp | Microsoft Docs
description: "Lär dig hur toouse Windows PowerShell-cmdlets i Azure RemoteApp."
services: remoteapp
documentationcenter: 
author: guscatalano
manager: mbaldwin
editor: 
ms.assetid: 7d3d5ded-6f73-4de6-a8ac-c1d622e842a2
ms.service: remoteapp
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 04/26/2017
ms.author: mbaldwin
ms.openlocfilehash: a09cae2093e2c3a4a2ed673b5d148a22ceb935f9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-windows-powershell-cmdlets-with-azure-remoteapp"></a>Använda Windows PowerShell-cmdlets med Azure RemoteApp
> [!IMPORTANT]
> Azure RemoteApp upphör att gälla den 31 augusti 2017. Läs hello [meddelande](https://go.microsoft.com/fwlink/?linkid=821148) mer information.
> 
> 

 Du kan använda hello Azure RemoteApp PowerShell-cmdlets tooadminister och underhålla din samlingar. Använd följande information tooget igång hello.

## <a name="get-hello-cmdlets"></a>Hämta hello-cmdlets
- - -
Först hämta hello Azure Powershell-cmdlets [här](http://go.microsoft.com/?linkid=9811175), hello RemoteApp-cmdlets som ingår i den. 

Kolla in hello [Azure RemoteApp-cmdlet-hjälpen](/powershell/module/azure?view=azuresmps-3.7.0).

## <a name="configure-azure-cmdlets-toouse-your-subscription"></a>Konfigurera Azure-cmdlets toouse din prenumeration
- - -
Följ [handboken](/powershell/azure/overview) så att du kan använda hello cmdlets mot dina Azure-prenumeration.

Du kan använda dessa steg tooget snabbt igång:

1. Hämta och installera hello [Azure PowerShell-cmdlets](http://go.microsoft.com/?linkid=9811175).
2. Starta Microsoft Azure PowerShell.
3. Kör **Add-AzureAccount** tooauthenticate tooyour Azure-prenumeration. När du uppmanas, anger hello samma användarnamn och lösenord du använder toosign i tooAzure portal.  
4. Kör **Get-AzureSubscription** toolist hello prenumerationer som är kopplade till ditt användarkonto. 
5. Kör **Välj AzureSubscription - SubscriptionName &lt;prenumerationsnamn&gt;**  eller **Välj AzureSubscription - SubscriptionId &lt;prenumerations-ID&gt;**  toospecify hello prenumeration toouse.

Grattis, Azure PowerShell-konsolen är konfigurerad och klar toouse. Tänk på att du behöver toorepeate steg 2 till 5 varje gång du startar hello hello Azure PowerShell-konsolen.  


## <a name="list-all-collections"></a>Visa en lista med alla samlingar
- - -
     Get-AzureRemoteAppCollection

## <a name="delete-a-collection"></a>Ta bort en samling
- - -
    Ta bort AzureRemoteAppCollection<enter collection name>

Exempel: `Remove-AzureRemoteAppCollection ContosoProduction`.

## <a name="create-a-cloud-collection"></a>Skapa en molnsamling
- - -
Det är enkelt, kör hello följande kommando:

    New-AzureRemoteAppCollection -Collectionname RAppO365Col1 -ImageName "Office 365 ProPlus (Subscription required)" -Plan Basic -Location "West US" - Description "Office 365 Collection."

Hej ovanför kommandot publiceras automatiskt Microsoft Office 365-program (Excel, OneNote, Outlook, PowerPoint, Visio och Word).

Skapa en samling kan ta 30 minuter eller längre toocomplete. Det här kommandot returnerar därför ett spårnings-ID som du kan använda på följande sätt:

    Get-AzureRemoteAppOperationResult -TrackingId xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx

Därefter hello samling du kan lägga till användare toohello samling med hello följande kommando:

    Add-AzureRemoteAppUser -CollectionName RAppO365Col1 -Type microsoftAccount -UserUpn someone@domain.com

Och du är klar! Att användaren ska kunna tooconnect toohello program med hjälp av hello Azure RemoteApp-klienten hitta [här](https://www.remoteapp.windowsazure.com/).

## <a name="available-cmdlets"></a>Tillgängliga cmdlet:ar
Det finns många andra kommandon som vi har, hello dokumentationen för dem kommer snart:

Grundläggande RemoteApp-samlingen cmdlets: 

* New-AzureRemoteAppCollection
* Get-AzureRemoteAppCollection
* Set-AzureRemoteAppCollection
* Update-AzureRemoteAppCollection
* Remove-AzureRemoteAppCollection
* Add-AzureRemoteAppUser
* Get-AzureRemoteAppUser
* Remove-AzureRemoteAppUser
* Get-AzureRemoteAppSession
* Disconnect-AzureRemoteAppSession
* Invoke-AzureRemoteAppSessionLogoff
* Send-AzureRemoteAppSessionMessage
* Get-AzureRemoteAppProgram
* Get-AzureRemoteAppStartMenuProgram
* Publish-AzureRemoteAppProgram
* Unpublish-AzureRemoteAppProgram
* Get-AzureRemoteAppCollectionUsageDetails
* Get-AzureRemoteAppCollectionUsageSummary
* Get-AzureRemoteAppPlan

RemoteApp-cmdlets för virtuellt nätverk:

* New-AzureRemoteAppVNet
* Get-AzureRemoteAppVNet
* Set-AzureRemoteAppVNet
* Remove-AzureRemoteAppVNet
* Get-AzureRemoteAppVpnDevice
* Get - AzureRemoteAppVpnDeviceConfigScript
* Reset-AzureRemoteAppVpnSharedKey

RemoteApp-mallen avbildningen cmdlets:

* New-AzureRemoteAppTemplateImage
* Get-AzureRemoteAppTemplateImage
* Rename-AzureRemoteAppTemplateImage
* Remove-AzureRemoteAppTemplateImage

Andra RemoteApp-cmdlets:

* Get-AzureRemoteAppLocation
* Get-AzureRemoteAppWorkspace
* Set-AzureRemoteAppWorkspace
* Get-AzureRemoteAppOperationResult

