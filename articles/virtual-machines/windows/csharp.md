---
title: "aaaCreate och hantera en Azure virtuella datorer med hjälp av C# | Microsoft Docs"
description: "Använd C# och Azure Resource Manager toodeploy en virtuell dator och alla dess stödfiler resurser."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: 8beeabde731bbaa25e68d2b9c5abbf71acbe377f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a><span data-ttu-id="fb866-103">Skapa och hantera virtuella Windows-datorer i Azure med C#</span><span class="sxs-lookup"><span data-stu-id="fb866-103">Create and manage Windows VMs in Azure using C#</span></span> #

<span data-ttu-id="fb866-104">En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fb866-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="fb866-105">Den här artikeln omfattar att skapa, hantera och ta bort VM-resurser med hjälp av C#.</span><span class="sxs-lookup"><span data-stu-id="fb866-105">This article covers creating, managing, and deleting VM resources using C#.</span></span> <span data-ttu-id="fb866-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="fb866-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="fb866-107">Skapa ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="fb866-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="fb866-108">Installera hello-paket</span><span class="sxs-lookup"><span data-stu-id="fb866-108">Install hello package</span></span>
> * <span data-ttu-id="fb866-109">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="fb866-109">Create credentials</span></span>
> * <span data-ttu-id="fb866-110">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="fb866-110">Create resources</span></span>
> * <span data-ttu-id="fb866-111">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="fb866-111">Perform management tasks</span></span>
> * <span data-ttu-id="fb866-112">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="fb866-112">Delete resources</span></span>
> * <span data-ttu-id="fb866-113">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="fb866-113">Run hello application</span></span>

<span data-ttu-id="fb866-114">Det tar cirka 20 minuter toodo dessa steg.</span><span class="sxs-lookup"><span data-stu-id="fb866-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="fb866-115">Skapa ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="fb866-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="fb866-116">Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="fb866-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="fb866-117">Välj **.NET skrivbord development** på hello arbetsbelastningar sidan och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="fb866-117">Select **.NET desktop development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="fb866-118">I hello sammanfattning, kan du se att **utvecklingsverktyg för .NET Framework 4 4.6** väljs automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="fb866-118">In hello summary, you can see that **.NET Framework 4 - 4.6 development tools** is automatically selected for you.</span></span> <span data-ttu-id="fb866-119">Om du redan har installerat Visual Studio kan du lägga till hello .NET arbetsbelastning med hello Visual Studio starta.</span><span class="sxs-lookup"><span data-stu-id="fb866-119">If you have already installed Visual Studio, you can add hello .NET workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="fb866-120">I Visual Studio klickar du på **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="fb866-120">In Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="fb866-121">I **mallar** > **Visual C#**väljer **Konsolapp (.NET Framework)**, ange *myDotnetProject* för hello namn Hej projektet, Välj hello platsen för hello-projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fb866-121">In **Templates** > **Visual C#**, select **Console App (.NET Framework)**, enter *myDotnetProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-hello-package"></a><span data-ttu-id="fb866-122">Installera hello-paket</span><span class="sxs-lookup"><span data-stu-id="fb866-122">Install hello package</span></span>

<span data-ttu-id="fb866-123">NuGet-paket är hello enklaste sättet tooinstall hello bibliotek som du behöver toofinish dessa steg.</span><span class="sxs-lookup"><span data-stu-id="fb866-123">NuGet packages are hello easiest way tooinstall hello libraries that you need toofinish these steps.</span></span> <span data-ttu-id="fb866-124">tooget hello bibliotek som du behöver i Visual Studio gör dessa steg:</span><span class="sxs-lookup"><span data-stu-id="fb866-124">tooget hello libraries that you need in Visual Studio, do these steps:</span></span>

1. <span data-ttu-id="fb866-125">Klicka på **verktyg** > **Nuget Package Manager**, och klicka sedan på **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="fb866-125">Click **Tools** > **Nuget Package Manager**, and then click **Package Manager Console**.</span></span>
2. <span data-ttu-id="fb866-126">Ange följande kommando i hello-konsolen:</span><span class="sxs-lookup"><span data-stu-id="fb866-126">Type this command in hello console:</span></span>

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a><span data-ttu-id="fb866-127">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="fb866-127">Create credentials</span></span>

<span data-ttu-id="fb866-128">Innan du startar det här steget, se till att du har åtkomst tooan [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb866-128">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="fb866-129">Du bör anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="fb866-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="fb866-130">Skapa hello auktoriseringsfilen</span><span class="sxs-lookup"><span data-stu-id="fb866-130">Create hello authorization file</span></span>

1. <span data-ttu-id="fb866-131">I Solution Explorer högerklickar du på *myDotnetProject* > **Lägg till** > **nytt objekt**, och välj sedan **textfil** i *Visual C# objekt*.</span><span class="sxs-lookup"><span data-stu-id="fb866-131">In Solution Explorer, right-click *myDotnetProject* > **Add** > **New Item**, and then select **Text File** in *Visual C# Items*.</span></span> <span data-ttu-id="fb866-132">Namnet hello filen *azureauth.properties*, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fb866-132">Name hello file *azureauth.properties*, and then click **Add**.</span></span>
2. <span data-ttu-id="fb866-133">Lägg till dessa auktorisering egenskaper:</span><span class="sxs-lookup"><span data-stu-id="fb866-133">Add these authorization properties:</span></span>

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    <span data-ttu-id="fb866-134">Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med hello Active Directory-program identifierare,  **&lt;autentiseringsnyckel&gt;**  med hello programmet nyckel och  **&lt;klient-id&gt;**  med hello-klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="fb866-134">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

3. <span data-ttu-id="fb866-135">Spara hello azureauth.properties filen.</span><span class="sxs-lookup"><span data-stu-id="fb866-135">Save hello azureauth.properties file.</span></span> 
4. <span data-ttu-id="fb866-136">Ange en miljövariabel i Windows som heter AZURE_AUTH_LOCATION med hello fullständig sökväg tooauthorization filen som du skapade.</span><span class="sxs-lookup"><span data-stu-id="fb866-136">Set an environment variable in Windows named AZURE_AUTH_LOCATION with hello full path tooauthorization file that you created.</span></span> <span data-ttu-id="fb866-137">Till exempel kan hello följande PowerShell-kommando användas:</span><span class="sxs-lookup"><span data-stu-id="fb866-137">For example, hello following PowerShell command can be used:</span></span>

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-hello-management-client"></a><span data-ttu-id="fb866-138">Skapa hello management-klienten</span><span class="sxs-lookup"><span data-stu-id="fb866-138">Create hello management client</span></span>

1. <span data-ttu-id="fb866-139">Öppna hello Program.cs-filen för hello-projekt som du skapade och Lägg sedan till dem med instruktioner toohello befintliga instruktioner överst i filen hello:</span><span class="sxs-lookup"><span data-stu-id="fb866-139">Open hello Program.cs file for hello project that you created, and then add these using statements toohello existing statements at top of hello file:</span></span>

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. <span data-ttu-id="fb866-140">toocreate hello management-klienten, lägger du till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-140">toocreate hello management client, add this code toohello Main method:</span></span>

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a><span data-ttu-id="fb866-141">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="fb866-141">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="fb866-142">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="fb866-142">Create hello resource group</span></span>

<span data-ttu-id="fb866-143">Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb866-143">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="fb866-144">toospecify som värden för hello programmet och skapa hello resursgrupp, lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-144">toospecify values for hello application and create hello resource group, add this code toohello Main method:</span></span>

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="fb866-145">Skapa hello tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="fb866-145">Create hello availability set</span></span>

<span data-ttu-id="fb866-146">[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare för dig toomaintain hello virtuella datorer som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="fb866-146">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="fb866-147">toocreate hello tillgänglighet ange, lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-147">toocreate hello availability set, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-hello-public-ip-address"></a><span data-ttu-id="fb866-148">Skapa hello offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="fb866-148">Create hello public IP address</span></span>

<span data-ttu-id="fb866-149">En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) är nödvändiga toocommunicate med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fb866-149">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="fb866-150">toocreate hello offentliga IP-adressen för hello virtuell dator, lägger du till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-150">toocreate hello public IP address for hello virtual machine, add this code toohello Main method:</span></span>
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="fb866-151">Skapa hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="fb866-151">Create hello virtual network</span></span>

<span data-ttu-id="fb866-152">En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb866-152">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="fb866-153">Lägg till den här koden toohello Main-metoden toocreate ett undernät och ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="fb866-153">toocreate a subnet and a virtual network, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-hello-network-interface"></a><span data-ttu-id="fb866-154">Skapa hello nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="fb866-154">Create hello network interface</span></span>

<span data-ttu-id="fb866-155">En virtuell dator måste en network interface toocommunicate hello virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="fb866-155">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="fb866-156">toocreate nätverksgränssnitt, Lägg till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-156">toocreate a network interface, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="fb866-157">Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="fb866-157">Create hello virtual machine</span></span>

<span data-ttu-id="fb866-158">Nu när du har skapat alla hello stöder resurser kan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fb866-158">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="fb866-159">toocreate Hej virtuella datorn, Lägg till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-159">toocreate hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> <span data-ttu-id="fb866-160">Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server för hello.</span><span class="sxs-lookup"><span data-stu-id="fb866-160">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="fb866-161">toolearn mer information om hur du väljer andra bilder, se [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fb866-161">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="fb866-162">Om du vill toouse en befintlig disk i stället för en marketplace-avbildning, använder du den här koden:</span><span class="sxs-lookup"><span data-stu-id="fb866-162">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span>

```
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a><span data-ttu-id="fb866-163">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="fb866-163">Perform management tasks</span></span>

<span data-ttu-id="fb866-164">Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="fb866-164">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="fb866-165">Dessutom kan du toocreate kod tooautomate repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="fb866-165">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="fb866-166">När du behöver toodo något med hello VM, måste du tooget en instans av den:</span><span class="sxs-lookup"><span data-stu-id="fb866-166">When you need toodo anything with hello VM, you need tooget an instance of it:</span></span>

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="fb866-167">Hämta information om hello VM</span><span class="sxs-lookup"><span data-stu-id="fb866-167">Get information about hello VM</span></span>

<span data-ttu-id="fb866-168">tooget information om hello virtuell dator, lägger du till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-168">tooget information about hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Getting information about hello virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="stop-hello-vm"></a><span data-ttu-id="fb866-169">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="fb866-169">Stop hello VM</span></span>

<span data-ttu-id="fb866-170">Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätta toobe debiteras för den eller stoppa en virtuell dator och frigör den.</span><span class="sxs-lookup"><span data-stu-id="fb866-170">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="fb866-171">När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.</span><span class="sxs-lookup"><span data-stu-id="fb866-171">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="fb866-172">toostop hello virtuell dator utan att det har frigjorts, Lägg till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-172">toostop hello virtual machine without deallocating it, add this code toohello Main method:</span></span>

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

<span data-ttu-id="fb866-173">Om du vill toodeallocate hello virtuella datorn, ändra hello avstängningsläge anropet toothis koden:</span><span class="sxs-lookup"><span data-stu-id="fb866-173">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```
vm.Deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="fb866-174">Starta hello VM</span><span class="sxs-lookup"><span data-stu-id="fb866-174">Start hello VM</span></span>

<span data-ttu-id="fb866-175">toostart Hej virtuella datorn, Lägg till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-175">toostart hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="fb866-176">Ändra storlek på hello VM</span><span class="sxs-lookup"><span data-stu-id="fb866-176">Resize hello VM</span></span>

<span data-ttu-id="fb866-177">Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fb866-177">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="fb866-178">Mer information finns i [VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="fb866-178">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="fb866-179">toochange storleken på hello virtuella datorn, Lägg till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-179">toochange size of hello virtual machine, add this code toohello Main method:</span></span>

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter toocontinue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="fb866-180">Lägg till en data disk toohello VM</span><span class="sxs-lookup"><span data-stu-id="fb866-180">Add a data disk toohello VM</span></span>

<span data-ttu-id="fb866-181">tooadd en data disk toohello virtuell dator, lägger du till den här koden toohello Main-metoden tooadd datadisk som är 2 GB i storlek, han LUN 0 och en cachelagring typ av ReadWrite:</span><span class="sxs-lookup"><span data-stu-id="fb866-181">tooadd a data disk toohello virtual machine, add this code toohello Main method tooadd a data disk that is 2 GB in size, han a LUN of 0 and a caching type of ReadWrite:</span></span>

```
Console.WriteLine("Adding data disk toovm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter toodelete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a><span data-ttu-id="fb866-182">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="fb866-182">Delete resources</span></span>

<span data-ttu-id="fb866-183">Eftersom debiteras du för resurser som används i Azure, men det är alltid bra toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="fb866-183">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="fb866-184">Om du vill toodelete hello virtuella datorer och alla hello stöder resurser, alla har toodo är delete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fb866-184">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

<span data-ttu-id="fb866-185">toodelete hello resurs och lägga till den här koden toohello Main-metoden:</span><span class="sxs-lookup"><span data-stu-id="fb866-185">toodelete hello resource group, add this code toohello Main method:</span></span>

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-hello-application"></a><span data-ttu-id="fb866-186">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="fb866-186">Run hello application</span></span>

<span data-ttu-id="fb866-187">Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish.</span><span class="sxs-lookup"><span data-stu-id="fb866-187">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> 

1. <span data-ttu-id="fb866-188">toorun hello-konsolprogram klickar du på **starta**.</span><span class="sxs-lookup"><span data-stu-id="fb866-188">toorun hello console application, click **Start**.</span></span>

2. <span data-ttu-id="fb866-189">Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify hello skapandet av hello resurser i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb866-189">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="fb866-190">Klicka på hello toosee information om Distributionsstatus om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="fb866-190">Click hello deployment status toosee information about hello deployment.</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb866-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb866-191">Next steps</span></span>
* <span data-ttu-id="fb866-192">Dra nytta av med hjälp av en mall toocreate en virtuell dator med hjälp av hello informationen i [distribuera ett Azure-dator med hjälp av C# och Resource Manager-mall](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fb866-192">Take advantage of using a template toocreate a virtual machine by using hello information in [Deploy an Azure Virtual Machine using C# and a Resource Manager template](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
* <span data-ttu-id="fb866-193">Lär dig mer om hur du använder hello [Azure-bibliotek för .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span><span class="sxs-lookup"><span data-stu-id="fb866-193">Learn more about using hello [Azure libraries for .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).</span></span>

