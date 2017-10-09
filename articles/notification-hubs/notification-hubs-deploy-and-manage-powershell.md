---
title: "aaaDeploy och hantera Notification Hubs med hjälp av PowerShell"
description: "Hur tooCreate och hantera Notification Hubs med PowerShell för automatisering"
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 8835bdefa0d360354263eab8040259ad08281771
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Distribuera och hantera Notification Hubs med PowerShell
## <a name="overview"></a>Översikt
Den här artikeln visar hur toouse skapa och hantera Azure Notification Hubs med hjälp av PowerShell. hello följande vanliga automation-aktiviteter som visas i det här avsnittet.

* Skapa en Meddelandehubb
* Ange autentiseringsuppgifter

Om du behöver också toocreate en ny service bus-namnrymd för notification Hub, se [hantera Service Bus med PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Hantera meddelanden nav stöds inte direkt av hello-cmdlets som ingår i Azure PowerShell. hello bäst från PowerShell är tooreference hello Microsoft.Azure.NotificationHubs.dll sammansättning. hello sammansättningen distribueras med hello [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Krav
Du måste ha hello följande innan du börjar den här artikeln:

* En Azure-prenumeration. Azure är en plattform som baseras på prenumerationer. Mer information om hur du skaffar en prenumeration finns [köpalternativ], [Medlemserbjudanden], eller [kostnadsfri utvärderingsversion].
* En dator med Azure PowerShell. Instruktioner finns i [installera och konfigurera Azure PowerShell].
* En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a>Inklusive en referens toohello .NET-sammansättning för Service Bus
Hantera Azure Notification Hubs är ännu inte medföljer hello PowerShell-cmdlets i Azure PowerShell. tooprovision meddelandehubbar som du kan använda hello .NET-klient som ingår i hello [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Kontrollera först att skriptet kan hitta hello **Microsoft.Azure.NotificationHubs.dll** sammansättning, som installeras som en NuGet-paketet i Visual Studio-projekt. I ordning toobe flexibla utför hello skript de här stegen:

1. Anger hello sökväg som anropades.
2. Går hello sökväg tills den hittar en mapp med namnet `packages`. Den här mappen skapas när du installerar NuGet-paket för Visual Studio-projekt.
3. Rekursivt söker hello `packages` mapp för en sammansättning med namnet **Microsoft.Azure.NotificationHubs.dll**.
4. Referenser hello sammansättningen så att hello typer är tillgängliga för senare användning.

Här är hur de här stegen implementeras i ett PowerShell-skript:

``` powershell

try
{
    # WARNING: Make sure tooreference hello latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding hello [Microsoft.Azure.NotificationHubs.dll] assembly toohello script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "hello [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added toohello script."
}

catch [System.Exception]
{
    Write-Error("Could not add hello Microsoft.Azure.NotificationHubs.dll assembly toohello script. Make sure you build hello solution before running hello provisioning script.")
}
```

## <a name="create-hello-namespacemanager-class"></a>Skapa hello NamespaceManager klass
tooprovision Notification Hubs, skapa en instans av hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klass från hello SDK. 

Du kan använda hello [Get-AzureSBAuthorizationRule] cmdlet som ingår i Azure PowerShell tooretrieve en auktoriseringsregel som har använt tooprovide en anslutningssträng. Vi lagrar en referens toohello `NamespaceManager` instans i hello `$NamespaceManager` variabeln. Vi använder `$NamespaceManager` tooprovision en meddelandehubb.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Etablera en ny Meddelandehubb
tooprovision en ny meddelandehubb använda hello [.NET-API för Meddelandehubbar].

I den här delen av hello skript ställa in fyra lokala variabler. 

1. `$Namespace`: Ange den här toohello namn för hello namnområde där du vill att toocreate en meddelandehubb.
2. `$Path`: Ange den här toohello sökvägsnamn hello ny meddelandehubb.  Till exempel ”MyHub”.    
3. `$WnsPackageSid`: Ange den här toohello paket-SID för du Windows-App från hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Ange den här toohello hemlig nyckel för du Windows-App från hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Dessa variabler används tooconnect tooyour namnområdet och skapa en ny Meddelandehubb konfigurerats toohandle Windows Notification Services (WNS) meddelanden med WNS autentiseringsuppgifterna för en Windows-App. Mer information om hur du skaffar hello paket-SID och hemlig nyckel finns hello [komma igång med Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kursen. 

* hello skript fragment använder hello `NamespaceManager` objekt toocheck toosee om hello Notification Hub identifieras av `$Path` finns.
* Om det inte finns hello skriptet skapar en `NotificationHubDescription` med WNS autentiseringsuppgifter och skicka den toohello `NamespaceManager` klassen `CreateNotificationHub` metod.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query hello namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if hello namespace already exists
if ($CurrentNamespace)
{
    Write-Output "hello namespace [$Namespace] in hello [$($CurrentNamespace.Region)] region was found."

    # Create hello NamespaceManager object used toocreate a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."

    # Check toosee if hello Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "hello [$Path] notification hub already exists in hello [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating hello [$Path] notification hub in hello [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "hello [$Path] notification hub was created in hello [$Namespace] namespace."
    }
}
else
{
    Write-Host "hello [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Ytterligare resurser
* [Hantera Service Bus med PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Hur toocreate Service Bus-köer, ämnen och prenumerationer med hjälp av ett PowerShell-skript](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Hur toocreate en Service Bus Namespace och en Händelsehubb med hjälp av ett PowerShell-skript](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Vissa färdiga skript är också tillgängliga för hämtning:

* [Service Bus PowerShell-skript](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[köpalternativ]: http://azure.microsoft.com/pricing/purchase-options/
[Medlemserbjudanden]: http://azure.microsoft.com/pricing/member-offers/
[kostnadsfri utvärderingsversion]: http://azure.microsoft.com/pricing/free-trial/
Installera och konfigurera [installera och konfigurera Azure PowerShell]: /powershell/azureps-cmdlets-docs
[.NET-API för Meddelandehubbar]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

