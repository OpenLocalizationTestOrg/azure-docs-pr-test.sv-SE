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
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a><span data-ttu-id="1c003-103">Distribuera och hantera Notification Hubs med PowerShell</span><span class="sxs-lookup"><span data-stu-id="1c003-103">Deploy and Manage Notification Hubs using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="1c003-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="1c003-104">Overview</span></span>
<span data-ttu-id="1c003-105">Den här artikeln visar hur toouse skapa och hantera Azure Notification Hubs med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c003-105">This article shows you how toouse Create and Manage Azure Notification Hubs using PowerShell.</span></span> <span data-ttu-id="1c003-106">hello följande vanliga automation-aktiviteter som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="1c003-106">hello following common automation tasks are shown in this topic.</span></span>

* <span data-ttu-id="1c003-107">Skapa en Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="1c003-107">Create a Notification Hub</span></span>
* <span data-ttu-id="1c003-108">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="1c003-108">Set Credentials</span></span>

<span data-ttu-id="1c003-109">Om du behöver också toocreate en ny service bus-namnrymd för notification Hub, se [hantera Service Bus med PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span><span class="sxs-lookup"><span data-stu-id="1c003-109">If you also need toocreate a new service bus namespace for your notification hubs, see [Manage Service Bus with PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span></span>

<span data-ttu-id="1c003-110">Hantera meddelanden nav stöds inte direkt av hello-cmdlets som ingår i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c003-110">Managing Notifications Hubs is not supported directly by hello cmdlets included with Azure PowerShell.</span></span> <span data-ttu-id="1c003-111">hello bäst från PowerShell är tooreference hello Microsoft.Azure.NotificationHubs.dll sammansättning.</span><span class="sxs-lookup"><span data-stu-id="1c003-111">hello best approach from PowerShell is tooreference hello Microsoft.Azure.NotificationHubs.dll assembly.</span></span> <span data-ttu-id="1c003-112">hello sammansättningen distribueras med hello [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="1c003-112">hello assembly is distributed with hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="1c003-113">Krav</span><span class="sxs-lookup"><span data-stu-id="1c003-113">Prerequisites</span></span>
<span data-ttu-id="1c003-114">Du måste ha hello följande innan du börjar den här artikeln:</span><span class="sxs-lookup"><span data-stu-id="1c003-114">Before you begin this article, you must have hello following:</span></span>

* <span data-ttu-id="1c003-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1c003-115">An Azure subscription.</span></span> <span data-ttu-id="1c003-116">Azure är en plattform som baseras på prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="1c003-116">Azure is a subscription-based platform.</span></span> <span data-ttu-id="1c003-117">Mer information om hur du skaffar en prenumeration finns [köpalternativ], [Medlemserbjudanden], eller [kostnadsfri utvärderingsversion].</span><span class="sxs-lookup"><span data-stu-id="1c003-117">For more information about obtaining a subscription, see [Purchase Options], [Member Offers], or [Free Trial].</span></span>
* <span data-ttu-id="1c003-118">En dator med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c003-118">A computer with Azure PowerShell.</span></span> <span data-ttu-id="1c003-119">Instruktioner finns i [installera och konfigurera Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="1c003-119">For instructions, see [Install and configure Azure PowerShell].</span></span>
* <span data-ttu-id="1c003-120">En förståelse av PowerShell-skript, NuGet-paket och hello .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="1c003-120">A general understanding of PowerShell scripts, NuGet packages, and hello .NET Framework.</span></span>

## <a name="including-a-reference-toohello-net-assembly-for-service-bus"></a><span data-ttu-id="1c003-121">Inklusive en referens toohello .NET-sammansättning för Service Bus</span><span class="sxs-lookup"><span data-stu-id="1c003-121">Including a reference toohello .NET assembly for Service Bus</span></span>
<span data-ttu-id="1c003-122">Hantera Azure Notification Hubs är ännu inte medföljer hello PowerShell-cmdlets i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="1c003-122">Managing Azure Notification Hubs is not yet included with hello PowerShell cmdlets in Azure PowerShell.</span></span> <span data-ttu-id="1c003-123">tooprovision meddelandehubbar som du kan använda hello .NET-klient som ingår i hello [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="1c003-123">tooprovision notification hubs, you can use hello .NET client provided in hello [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="1c003-124">Kontrollera först att skriptet kan hitta hello **Microsoft.Azure.NotificationHubs.dll** sammansättning, som installeras som en NuGet-paketet i Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="1c003-124">First, make sure your script can locate hello **Microsoft.Azure.NotificationHubs.dll** assembly, which is installed as a NuGet package in a Visual Studio project.</span></span> <span data-ttu-id="1c003-125">I ordning toobe flexibla utför hello skript de här stegen:</span><span class="sxs-lookup"><span data-stu-id="1c003-125">In order toobe flexible, hello script performs these steps:</span></span>

1. <span data-ttu-id="1c003-126">Anger hello sökväg som anropades.</span><span class="sxs-lookup"><span data-stu-id="1c003-126">Determines hello path at which it was invoked.</span></span>
2. <span data-ttu-id="1c003-127">Går hello sökväg tills den hittar en mapp med namnet `packages`.</span><span class="sxs-lookup"><span data-stu-id="1c003-127">Traverses hello path until it finds a folder named `packages`.</span></span> <span data-ttu-id="1c003-128">Den här mappen skapas när du installerar NuGet-paket för Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="1c003-128">This folder is created when you install NuGet packages for Visual Studio projects.</span></span>
3. <span data-ttu-id="1c003-129">Rekursivt söker hello `packages` mapp för en sammansättning med namnet **Microsoft.Azure.NotificationHubs.dll**.</span><span class="sxs-lookup"><span data-stu-id="1c003-129">Recursively searches hello `packages` folder for an assembly named **Microsoft.Azure.NotificationHubs.dll**.</span></span>
4. <span data-ttu-id="1c003-130">Referenser hello sammansättningen så att hello typer är tillgängliga för senare användning.</span><span class="sxs-lookup"><span data-stu-id="1c003-130">References hello assembly so that hello types are available for later use.</span></span>

<span data-ttu-id="1c003-131">Här är hur de här stegen implementeras i ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="1c003-131">Here's how these steps are implemented in a PowerShell script:</span></span>

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

## <a name="create-hello-namespacemanager-class"></a><span data-ttu-id="1c003-132">Skapa hello NamespaceManager klass</span><span class="sxs-lookup"><span data-stu-id="1c003-132">Create hello NamespaceManager class</span></span>
<span data-ttu-id="1c003-133">tooprovision Notification Hubs, skapa en instans av hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klass från hello SDK.</span><span class="sxs-lookup"><span data-stu-id="1c003-133">tooprovision Notification Hubs, create an instance of hello [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) class from hello SDK.</span></span> 

<span data-ttu-id="1c003-134">Du kan använda hello [Get-AzureSBAuthorizationRule] cmdlet som ingår i Azure PowerShell tooretrieve en auktoriseringsregel som har använt tooprovide en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="1c003-134">You can use hello [Get-AzureSBAuthorizationRule] cmdlet included with Azure PowerShell tooretrieve an authorization rule that's used tooprovide a connection string.</span></span> <span data-ttu-id="1c003-135">Vi lagrar en referens toohello `NamespaceManager` instans i hello `$NamespaceManager` variabeln.</span><span class="sxs-lookup"><span data-stu-id="1c003-135">We'll store a reference toohello `NamespaceManager` instance in hello `$NamespaceManager` variable.</span></span> <span data-ttu-id="1c003-136">Vi använder `$NamespaceManager` tooprovision en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="1c003-136">We will use `$NamespaceManager` tooprovision a notification hub.</span></span>

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create hello NamespaceManager object toocreate hello hub
Write-Output "Creating a NamespaceManager object for hello [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for hello [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a><span data-ttu-id="1c003-137">Etablera en ny Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="1c003-137">Provisioning a new Notification Hub</span></span>
<span data-ttu-id="1c003-138">tooprovision en ny meddelandehubb använda hello [.NET-API för Meddelandehubbar].</span><span class="sxs-lookup"><span data-stu-id="1c003-138">tooprovision a new notification hub, use hello [.NET API for Notification Hubs].</span></span>

<span data-ttu-id="1c003-139">I den här delen av hello skript ställa in fyra lokala variabler.</span><span class="sxs-lookup"><span data-stu-id="1c003-139">In this part of hello script you set up four local variables.</span></span> 

1. <span data-ttu-id="1c003-140">`$Namespace`: Ange den här toohello namn för hello namnområde där du vill att toocreate en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="1c003-140">`$Namespace` : Set this toohello name of hello namespace where you want toocreate a notification hub.</span></span>
2. <span data-ttu-id="1c003-141">`$Path`: Ange den här toohello sökvägsnamn hello ny meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="1c003-141">`$Path` : Set this path toohello name of hello new notification hub.</span></span>  <span data-ttu-id="1c003-142">Till exempel ”MyHub”.</span><span class="sxs-lookup"><span data-stu-id="1c003-142">For example, "MyHub".</span></span>    
3. <span data-ttu-id="1c003-143">`$WnsPackageSid`: Ange den här toohello paket-SID för du Windows-App från hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1c003-143">`$WnsPackageSid` : Set this toohello package SID for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>
4. <span data-ttu-id="1c003-144">`$WnsSecretkey`: Ange den här toohello hemlig nyckel för du Windows-App från hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="1c003-144">`$WnsSecretkey`: Set this toohello secret key for you Windows App from hello [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>

<span data-ttu-id="1c003-145">Dessa variabler används tooconnect tooyour namnområdet och skapa en ny Meddelandehubb konfigurerats toohandle Windows Notification Services (WNS) meddelanden med WNS autentiseringsuppgifterna för en Windows-App.</span><span class="sxs-lookup"><span data-stu-id="1c003-145">These variables are used tooconnect tooyour namespace and create a new Notification Hub configured toohandle Windows Notification Services (WNS) notifications with WNS credentials for a Windows App.</span></span> <span data-ttu-id="1c003-146">Mer information om hur du skaffar hello paket-SID och hemlig nyckel finns hello [komma igång med Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="1c003-146">For information on obtaining hello package SID and secret key see, hello [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span> 

* <span data-ttu-id="1c003-147">hello skript fragment använder hello `NamespaceManager` objekt toocheck toosee om hello Notification Hub identifieras av `$Path` finns.</span><span class="sxs-lookup"><span data-stu-id="1c003-147">hello script snippet uses hello `NamespaceManager` object toocheck toosee if hello Notification Hub identified by `$Path` exists.</span></span>
* <span data-ttu-id="1c003-148">Om det inte finns hello skriptet skapar en `NotificationHubDescription` med WNS autentiseringsuppgifter och skicka den toohello `NamespaceManager` klassen `CreateNotificationHub` metod.</span><span class="sxs-lookup"><span data-stu-id="1c003-148">If it does not exist, hello script will create an `NotificationHubDescription` with WNS credentials and pass that toohello `NamespaceManager` class `CreateNotificationHub` method.</span></span>

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




## <a name="additional-resources"></a><span data-ttu-id="1c003-149">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="1c003-149">Additional Resources</span></span>
* [<span data-ttu-id="1c003-150">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="1c003-150">Manage Service Bus with PowerShell</span></span>](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="1c003-151">Hur toocreate Service Bus-köer, ämnen och prenumerationer med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1c003-151">How toocreate Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="1c003-152">Hur toocreate en Service Bus Namespace och en Händelsehubb med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1c003-152">How toocreate a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

<span data-ttu-id="1c003-153">Vissa färdiga skript är också tillgängliga för hämtning:</span><span class="sxs-lookup"><span data-stu-id="1c003-153">Some ready-made scripts are also available for download:</span></span>

* [<span data-ttu-id="1c003-154">Service Bus PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="1c003-154">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[köpalternativ]: http://azure.microsoft.com/pricing/purchase-options/
[Medlemserbjudanden]: http://azure.microsoft.com/pricing/member-offers/
[kostnadsfri utvärderingsversion]: http://azure.microsoft.com/pricing/free-trial/
Installera och konfigurera [installera och konfigurera Azure PowerShell]: /powershell/azureps-cmdlets-docs
[.NET-API för Meddelandehubbar]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx

