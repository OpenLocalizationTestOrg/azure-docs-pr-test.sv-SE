---
title: Distribuera och hantera Notification Hubs med PowerShell
description: "Hur du skapar och hanterar Meddelandehubbar som använder PowerShell för Automation"
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
ms.openlocfilehash: 4db058e4bd91dc287b14e887abc6c378c65c4a2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a><span data-ttu-id="2de75-103">Distribuera och hantera Notification Hubs med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2de75-103">Deploy and Manage Notification Hubs using PowerShell</span></span>
## <a name="overview"></a><span data-ttu-id="2de75-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="2de75-104">Overview</span></span>
<span data-ttu-id="2de75-105">Den här artikeln visar hur du använder skapa och hantera Azure Notification Hubs med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2de75-105">This article shows you how to use Create and Manage Azure Notification Hubs using PowerShell.</span></span> <span data-ttu-id="2de75-106">Följande vanliga automation-aktiviteter som visas i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="2de75-106">The following common automation tasks are shown in this topic.</span></span>

* <span data-ttu-id="2de75-107">Skapa en Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="2de75-107">Create a Notification Hub</span></span>
* <span data-ttu-id="2de75-108">Ange autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="2de75-108">Set Credentials</span></span>

<span data-ttu-id="2de75-109">Om du måste också skapa en ny service bus-namnrymd för notification Hub, se [hantera Service Bus med PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span><span class="sxs-lookup"><span data-stu-id="2de75-109">If you also need to create a new service bus namespace for your notification hubs, see [Manage Service Bus with PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).</span></span>

<span data-ttu-id="2de75-110">Hantera meddelanden nav stöds inte direkt av de cmdletar som ingår i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2de75-110">Managing Notifications Hubs is not supported directly by the cmdlets included with Azure PowerShell.</span></span> <span data-ttu-id="2de75-111">Det bästa sättet från PowerShell är att referera till Microsoft.Azure.NotificationHubs.dll-sammansättningen.</span><span class="sxs-lookup"><span data-stu-id="2de75-111">The best approach from PowerShell is to reference the Microsoft.Azure.NotificationHubs.dll assembly.</span></span> <span data-ttu-id="2de75-112">Sammansättningen distribueras med den [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="2de75-112">The assembly is distributed with the [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="2de75-113">Krav</span><span class="sxs-lookup"><span data-stu-id="2de75-113">Prerequisites</span></span>
<span data-ttu-id="2de75-114">Innan du påbörjar den här artikeln måste du ha:</span><span class="sxs-lookup"><span data-stu-id="2de75-114">Before you begin this article, you must have the following:</span></span>

* <span data-ttu-id="2de75-115">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="2de75-115">An Azure subscription.</span></span> <span data-ttu-id="2de75-116">Azure är en plattform som baseras på prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2de75-116">Azure is a subscription-based platform.</span></span> <span data-ttu-id="2de75-117">Mer information om hur du skaffar en prenumeration finns [köpalternativ], [Medlemserbjudanden], eller [kostnadsfri utvärderingsversion].</span><span class="sxs-lookup"><span data-stu-id="2de75-117">For more information about obtaining a subscription, see [Purchase Options], [Member Offers], or [Free Trial].</span></span>
* <span data-ttu-id="2de75-118">En dator med Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2de75-118">A computer with Azure PowerShell.</span></span> <span data-ttu-id="2de75-119">Instruktioner finns i [installera och konfigurera Azure PowerShell].</span><span class="sxs-lookup"><span data-stu-id="2de75-119">For instructions, see [Install and configure Azure PowerShell].</span></span>
* <span data-ttu-id="2de75-120">En förståelse av PowerShell-skript, NuGet-paket och .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="2de75-120">A general understanding of PowerShell scripts, NuGet packages, and the .NET Framework.</span></span>

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a><span data-ttu-id="2de75-121">Inklusive en referens till .NET-sammansättning för Service Bus</span><span class="sxs-lookup"><span data-stu-id="2de75-121">Including a reference to the .NET assembly for Service Bus</span></span>
<span data-ttu-id="2de75-122">Hantera Azure Notification Hubs ingår inte ännu i PowerShell-cmdlets i Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="2de75-122">Managing Azure Notification Hubs is not yet included with the PowerShell cmdlets in Azure PowerShell.</span></span> <span data-ttu-id="2de75-123">Om du vill etablera meddelandehubbar, du kan använda .NET-klienten som angetts i den [Microsoft Azure Notification Hubs NuGet-paketet](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="2de75-123">To provision notification hubs, you can use the .NET client provided in the [Microsoft Azure Notification Hubs NuGet package](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>

<span data-ttu-id="2de75-124">Kontrollera först att skriptet kan hitta den **Microsoft.Azure.NotificationHubs.dll** sammansättning, som installeras som en NuGet-paketet i Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="2de75-124">First, make sure your script can locate the **Microsoft.Azure.NotificationHubs.dll** assembly, which is installed as a NuGet package in a Visual Studio project.</span></span> <span data-ttu-id="2de75-125">För att vara flexibel måste utför skriptet de här stegen:</span><span class="sxs-lookup"><span data-stu-id="2de75-125">In order to be flexible, the script performs these steps:</span></span>

1. <span data-ttu-id="2de75-126">Anger sökvägen som anropades.</span><span class="sxs-lookup"><span data-stu-id="2de75-126">Determines the path at which it was invoked.</span></span>
2. <span data-ttu-id="2de75-127">Passerar sökvägen tills den hittar en mapp med namnet `packages`.</span><span class="sxs-lookup"><span data-stu-id="2de75-127">Traverses the path until it finds a folder named `packages`.</span></span> <span data-ttu-id="2de75-128">Den här mappen skapas när du installerar NuGet-paket för Visual Studio-projekt.</span><span class="sxs-lookup"><span data-stu-id="2de75-128">This folder is created when you install NuGet packages for Visual Studio projects.</span></span>
3. <span data-ttu-id="2de75-129">Rekursivt söker den `packages` mapp för en sammansättning med namnet **Microsoft.Azure.NotificationHubs.dll**.</span><span class="sxs-lookup"><span data-stu-id="2de75-129">Recursively searches the `packages` folder for an assembly named **Microsoft.Azure.NotificationHubs.dll**.</span></span>
4. <span data-ttu-id="2de75-130">Refererar till sammansättningen så att typerna som är tillgängliga för senare användning.</span><span class="sxs-lookup"><span data-stu-id="2de75-130">References the assembly so that the types are available for later use.</span></span>

<span data-ttu-id="2de75-131">Här är hur de här stegen implementeras i ett PowerShell-skript:</span><span class="sxs-lookup"><span data-stu-id="2de75-131">Here's how these steps are implemented in a PowerShell script:</span></span>

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a><span data-ttu-id="2de75-132">Skapa NamespaceManager-klass</span><span class="sxs-lookup"><span data-stu-id="2de75-132">Create the NamespaceManager class</span></span>
<span data-ttu-id="2de75-133">För att etablera Notification Hubs för att skapa en instans av den [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klass från SDK.</span><span class="sxs-lookup"><span data-stu-id="2de75-133">To provision Notification Hubs, create an instance of the [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) class from the SDK.</span></span> 

<span data-ttu-id="2de75-134">Du kan använda den [Get-AzureSBAuthorizationRule] cmdlet som ingår i Azure PowerShell för att hämta en auktoriseringsregel som används för att ange en anslutningssträng.</span><span class="sxs-lookup"><span data-stu-id="2de75-134">You can use the [Get-AzureSBAuthorizationRule] cmdlet included with Azure PowerShell to retrieve an authorization rule that's used to provide a connection string.</span></span> <span data-ttu-id="2de75-135">Vi kommer att lagra en referens till den `NamespaceManager` instans i den `$NamespaceManager` variabeln.</span><span class="sxs-lookup"><span data-stu-id="2de75-135">We'll store a reference to the `NamespaceManager` instance in the `$NamespaceManager` variable.</span></span> <span data-ttu-id="2de75-136">Vi använder `$NamespaceManager` att etablera en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="2de75-136">We will use `$NamespaceManager` to provision a notification hub.</span></span>

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a><span data-ttu-id="2de75-137">Etablera en ny Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="2de75-137">Provisioning a new Notification Hub</span></span>
<span data-ttu-id="2de75-138">För att etablera en ny meddelandehubb, Använd den [.NET-API för Meddelandehubbar].</span><span class="sxs-lookup"><span data-stu-id="2de75-138">To provision a new notification hub, use the [.NET API for Notification Hubs].</span></span>

<span data-ttu-id="2de75-139">I den här delen av skriptet kan du ställa in fyra lokala variabler.</span><span class="sxs-lookup"><span data-stu-id="2de75-139">In this part of the script you set up four local variables.</span></span> 

1. <span data-ttu-id="2de75-140">`$Namespace`: Ange namnet på namnområdet som du vill skapa en meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="2de75-140">`$Namespace` : Set this to the name of the namespace where you want to create a notification hub.</span></span>
2. <span data-ttu-id="2de75-141">`$Path`: Ange den här sökvägen till namnet på den nya meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="2de75-141">`$Path` : Set this path to the name of the new notification hub.</span></span>  <span data-ttu-id="2de75-142">Till exempel ”MyHub”.</span><span class="sxs-lookup"><span data-stu-id="2de75-142">For example, "MyHub".</span></span>    
3. <span data-ttu-id="2de75-143">`$WnsPackageSid`: Ställ in på paket-SID för Windows-App från den [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2de75-143">`$WnsPackageSid` : Set this to the package SID for you Windows App from the [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>
4. <span data-ttu-id="2de75-144">`$WnsSecretkey`: Ställ in på den hemliga nyckeln för Windows-App från den [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span><span class="sxs-lookup"><span data-stu-id="2de75-144">`$WnsSecretkey`: Set this to the secret key for you Windows App from the [Windows Dev Center](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).</span></span>

<span data-ttu-id="2de75-145">Dessa variabler används för att ansluta till namnområdet och skapa en ny Meddelandehubb som konfigurerats för att hantera Windows Notification Services (WNS) meddelanden med WNS autentiseringsuppgifter för en Windows-App.</span><span class="sxs-lookup"><span data-stu-id="2de75-145">These variables are used to connect to your namespace and create a new Notification Hub configured to handle Windows Notification Services (WNS) notifications with WNS credentials for a Windows App.</span></span> <span data-ttu-id="2de75-146">Mer information om hur du skaffar paketet SID och hemlig nyckel finns i [komma igång med Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="2de75-146">For information on obtaining the package SID and secret key see, the [Getting Started with Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) tutorial.</span></span> 

* <span data-ttu-id="2de75-147">Skriptet fragment använder den `NamespaceManager` objekt att kontrollera om Meddelandehubben som identifieras av `$Path` finns.</span><span class="sxs-lookup"><span data-stu-id="2de75-147">The script snippet uses the `NamespaceManager` object to check to see if the Notification Hub identified by `$Path` exists.</span></span>
* <span data-ttu-id="2de75-148">Om det inte finns skriptet skapar en `NotificationHubDescription` med WNS autentiseringsuppgifter och klara att för att den `NamespaceManager` klassen `CreateNotificationHub` metod.</span><span class="sxs-lookup"><span data-stu-id="2de75-148">If it does not exist, the script will create an `NotificationHubDescription` with WNS credentials and pass that to the `NamespaceManager` class `CreateNotificationHub` method.</span></span>

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a><span data-ttu-id="2de75-149">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="2de75-149">Additional Resources</span></span>
* [<span data-ttu-id="2de75-150">Hantera Service Bus med PowerShell</span><span class="sxs-lookup"><span data-stu-id="2de75-150">Manage Service Bus with PowerShell</span></span>](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [<span data-ttu-id="2de75-151">Så här skapar du Service Bus-köer, ämnen och prenumerationer med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="2de75-151">How to create Service Bus queues, topics and subscriptions using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [<span data-ttu-id="2de75-152">Så här skapar du en Service Bus-Namespace och en Händelsehubb med hjälp av ett PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="2de75-152">How to create a Service Bus Namespace and an Event Hub using a PowerShell script</span></span>](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

<span data-ttu-id="2de75-153">Vissa färdiga skript är också tillgängliga för hämtning:</span><span class="sxs-lookup"><span data-stu-id="2de75-153">Some ready-made scripts are also available for download:</span></span>

* [<span data-ttu-id="2de75-154">Service Bus PowerShell-skript</span><span class="sxs-lookup"><span data-stu-id="2de75-154">Service Bus PowerShell Scripts</span></span>](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

<span data-ttu-id="2de75-155">[köpalternativ]: http://azure.microsoft.com/pricing/purchase-options/</span><span class="sxs-lookup"><span data-stu-id="2de75-155">[Purchase Options]: http://azure.microsoft.com/pricing/purchase-options/</span></span>
<span data-ttu-id="2de75-156">[Medlemserbjudanden]: http://azure.microsoft.com/pricing/member-offers/</span><span class="sxs-lookup"><span data-stu-id="2de75-156">[Member Offers]: http://azure.microsoft.com/pricing/member-offers/</span></span>
<span data-ttu-id="2de75-157">[kostnadsfri utvärderingsversion]: http://azure.microsoft.com/pricing/free-trial/</span><span class="sxs-lookup"><span data-stu-id="2de75-157">[Free Trial]: http://azure.microsoft.com/pricing/free-trial/</span></span>
<span data-ttu-id="2de75-158">Installera och konfigurera [installera och konfigurera Azure PowerShell]: /powershell/azureps-cmdlets-docs</span><span class="sxs-lookup"><span data-stu-id="2de75-158">[Install and configure Azure PowerShell]: /powershell/azureps-cmdlets-docs</span></span>
<span data-ttu-id="2de75-159">[.NET-API för Meddelandehubbar]: https://msdn.microsoft.com/library/azure/mt414893.aspx</span><span class="sxs-lookup"><span data-stu-id="2de75-159">[.NET API for Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx</span></span>
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
<span data-ttu-id="2de75-160">[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx</span><span class="sxs-lookup"><span data-stu-id="2de75-160">[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx</span></span>

