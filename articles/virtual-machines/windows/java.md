---
title: Skapa och hantera en virtuell Azure-dator med Java | Microsoft Docs
description: "Använd Java och Azure Resource Manager för att distribuera en virtuell dator och alla dess stödfiler resurser."
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b9e739a07c5863577285fb3a221b372b385c6762
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a><span data-ttu-id="b5a08-103">Skapa och hantera virtuella Windows-datorer i Azure som använder Java</span><span class="sxs-lookup"><span data-stu-id="b5a08-103">Create and manage Windows VMs in Azure using Java</span></span>

<span data-ttu-id="b5a08-104">En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="b5a08-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="b5a08-105">Den här artikeln beskriver hur du skapar, hantera och ta bort VM-resurser med hjälp av Java.</span><span class="sxs-lookup"><span data-stu-id="b5a08-105">This article covers creating, managing, and deleting VM resources using Java.</span></span> <span data-ttu-id="b5a08-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="b5a08-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="b5a08-107">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="b5a08-107">Create a Maven project</span></span>
> * <span data-ttu-id="b5a08-108">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="b5a08-108">Add dependencies</span></span>
> * <span data-ttu-id="b5a08-109">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b5a08-109">Create credentials</span></span>
> * <span data-ttu-id="b5a08-110">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="b5a08-110">Create resources</span></span>
> * <span data-ttu-id="b5a08-111">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="b5a08-111">Perform management tasks</span></span>
> * <span data-ttu-id="b5a08-112">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="b5a08-112">Delete resources</span></span>
> * <span data-ttu-id="b5a08-113">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="b5a08-113">Run the application</span></span>

<span data-ttu-id="b5a08-114">Det tar ungefär 20 minuter för att utföra de här stegen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-114">It takes about 20 minutes to do these steps.</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="b5a08-115">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="b5a08-115">Create a Maven project</span></span>

1. <span data-ttu-id="b5a08-116">Om du inte redan har gjort det installerar [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="b5a08-116">If you haven't already done so, install [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
2. <span data-ttu-id="b5a08-117">Installera [Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="b5a08-117">Install [Maven](http://maven.apache.org/download.cgi).</span></span>
3. <span data-ttu-id="b5a08-118">Skapa en ny mapp och projektet:</span><span class="sxs-lookup"><span data-stu-id="b5a08-118">Create a new folder and the project:</span></span>
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a><span data-ttu-id="b5a08-119">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="b5a08-119">Add dependencies</span></span>

1. <span data-ttu-id="b5a08-120">Under den `testAzureApp` mapp, öppna den `pom.xml` och Lägg till versionskonfiguration till &lt;projekt&gt; att aktivera programmet:</span><span class="sxs-lookup"><span data-stu-id="b5a08-120">Under the `testAzureApp` folder, open the `pom.xml` file and add the build configuration to &lt;project&gt; to enable the building of your application:</span></span>

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. <span data-ttu-id="b5a08-121">Lägga till beroenden som behövs för att komma åt Azure Java SDK.</span><span class="sxs-lookup"><span data-stu-id="b5a08-121">Add the dependencies that are needed to access the Azure Java SDK.</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency> 
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. <span data-ttu-id="b5a08-122">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-122">Save the file.</span></span>

## <a name="create-credentials"></a><span data-ttu-id="b5a08-123">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="b5a08-123">Create credentials</span></span>

<span data-ttu-id="b5a08-124">Innan du startar det här steget, se till att du har åtkomst till en [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b5a08-124">Before you start this step, make sure that you have access to an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="b5a08-125">Du bör anteckna det program-ID, autentiseringsnyckeln och klient-ID som du behöver i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="b5a08-125">You should also record the application ID, the authentication key, and the tenant ID that you need in a later step.</span></span>

### <a name="create-the-authorization-file"></a><span data-ttu-id="b5a08-126">Skapa auktoriseringsfilen</span><span class="sxs-lookup"><span data-stu-id="b5a08-126">Create the authorization file</span></span>

1. <span data-ttu-id="b5a08-127">Skapa en fil med namnet `azureauth.properties` och Lägg till följande egenskaper:</span><span class="sxs-lookup"><span data-stu-id="b5a08-127">Create a file named `azureauth.properties` and add these properties to it:</span></span>

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

    <span data-ttu-id="b5a08-128">Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med programidentifierare Active Directory  **&lt;autentiseringsnyckel&gt;**  med nyckeln för programmet och  **&lt;klient-id&gt;**  med klient-ID.</span><span class="sxs-lookup"><span data-stu-id="b5a08-128">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with the Active Directory application identifier, **&lt;authentication-key&gt;** with the application key, and **&lt;tenant-id&gt;** with the tenant identifier.</span></span>

2. <span data-ttu-id="b5a08-129">Spara filen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-129">Save the file.</span></span>
3. <span data-ttu-id="b5a08-130">Ange miljövariabeln AZURE_AUTH_LOCATION i gränssnittet med den fullständiga sökvägen till autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-130">Set an environment variable named AZURE_AUTH_LOCATION in your shell with the full path to the authentication file.</span></span>

### <a name="create-the-management-client"></a><span data-ttu-id="b5a08-131">Skapa management-klienten</span><span class="sxs-lookup"><span data-stu-id="b5a08-131">Create the management client</span></span>

1. <span data-ttu-id="b5a08-132">Öppna den `App.java` filen `src\main\java\com\fabrikam` och kontrollera att det här paketet-instruktionen är längst upp:</span><span class="sxs-lookup"><span data-stu-id="b5a08-132">Open the `App.java` file under `src\main\java\com\fabrikam` and make sure this package statement is at the top:</span></span>

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. <span data-ttu-id="b5a08-133">Lägg till dessa under instruktionen paketet importera instruktioner:</span><span class="sxs-lookup"><span data-stu-id="b5a08-133">Under the package statement, add these import statements:</span></span>
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. <span data-ttu-id="b5a08-134">Lägg till den här koden main-metoden i klassen App för att skapa Active Directory-autentiseringsuppgifter som du behöver göra begäranden:</span><span class="sxs-lookup"><span data-stu-id="b5a08-134">To create the Active Directory credentials that you need to make requests, add this code to the main method of the App class:</span></span>
   
    ```java
    try {    
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a><span data-ttu-id="b5a08-135">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="b5a08-135">Create resources</span></span>

### <a name="create-the-resource-group"></a><span data-ttu-id="b5a08-136">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b5a08-136">Create the resource group</span></span>

<span data-ttu-id="b5a08-137">Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b5a08-137">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="b5a08-138">Ange värden för programmet och skapa resursgruppen genom att lägga till den här koden i try-block i main-metoden:</span><span class="sxs-lookup"><span data-stu-id="b5a08-138">To specify values for the application and create the resource group, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a><span data-ttu-id="b5a08-139">Skapa tillgänglighetsuppsättningen</span><span class="sxs-lookup"><span data-stu-id="b5a08-139">Create the availability set</span></span>

<span data-ttu-id="b5a08-140">[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare att underhålla de virtuella datorerna som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="b5a08-140">[Availability sets](tutorial-availability-sets.md) make it easier for you to maintain the virtual machines used by your application.</span></span>

<span data-ttu-id="b5a08-141">Lägg till den här koden try-block i main-metoden för att skapa tillgänglighetsuppsättningen:</span><span class="sxs-lookup"><span data-stu-id="b5a08-141">To create the availability set, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-the-public-ip-address"></a><span data-ttu-id="b5a08-142">Skapa offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="b5a08-142">Create the public IP address</span></span>

<span data-ttu-id="b5a08-143">En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) behövs för att kommunicera med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5a08-143">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed to communicate with the virtual machine.</span></span>

<span data-ttu-id="b5a08-144">Lägg till den här koden try-block i main-metoden för att skapa den offentliga IP-adressen för den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="b5a08-144">To create the public IP address for the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a><span data-ttu-id="b5a08-145">Skapa virtuella nätverk</span><span class="sxs-lookup"><span data-stu-id="b5a08-145">Create the virtual network</span></span>

<span data-ttu-id="b5a08-146">En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b5a08-146">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="b5a08-147">Lägg till den här koden try-block i main-metoden för att skapa ett undernät och ett virtuellt nätverk:</span><span class="sxs-lookup"><span data-stu-id="b5a08-147">To create a subnet and a virtual network, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a><span data-ttu-id="b5a08-148">Skapa nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="b5a08-148">Create the network interface</span></span>

<span data-ttu-id="b5a08-149">En virtuell dator måste ett nätverksgränssnitt för att kommunicera på det virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="b5a08-149">A virtual machine needs a network interface to communicate on the virtual network.</span></span>

<span data-ttu-id="b5a08-150">Lägg till den här koden try-block i main-metoden för att skapa ett nätverksgränssnitt:</span><span class="sxs-lookup"><span data-stu-id="b5a08-150">To create a network interface, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a><span data-ttu-id="b5a08-151">Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-151">Create the virtual machine</span></span>

<span data-ttu-id="b5a08-152">Nu när du har skapat alla stödresurser kan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b5a08-152">Now that you created all the supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="b5a08-153">Lägg till den här koden try-block i main-metoden för att skapa den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="b5a08-153">To create the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> <span data-ttu-id="b5a08-154">Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server.</span><span class="sxs-lookup"><span data-stu-id="b5a08-154">This tutorial creates a virtual machine running a version of the Windows Server operating system.</span></span> <span data-ttu-id="b5a08-155">Läs mer om att välja andra bilder i [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b5a08-155">To learn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and the Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="b5a08-156">Om du vill använda en befintlig disk i stället för en marketplace-avbildning, Använd den här koden:</span><span class="sxs-lookup"><span data-stu-id="b5a08-156">If you want to use an existing disk instead of a marketplace image, use this code:</span></span> 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd") 
    .withSizeInGB(128) 
    .withSku(DiskSkuTypes.PremiumLRS) 
    .create(); 

azure.virtualMachines.define("myVM") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withExistingPrimaryNetworkInterface(networkInterface) 
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows) 
    .withExistingAvailabilitySet(availabilitySet) 
    .withSize(VirtualMachineSizeTypes.StandardDS1) 
    .create(); 
``` 

## <a name="perform-management-tasks"></a><span data-ttu-id="b5a08-157">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="b5a08-157">Perform management tasks</span></span>

<span data-ttu-id="b5a08-158">Under livscykeln för en virtuell dator kan du vill köra hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b5a08-158">During the lifecycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="b5a08-159">Dessutom kanske du vill skapa kod för att automatisera repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="b5a08-159">Additionally, you may want to create code to automate repetitive or complex tasks.</span></span>

<span data-ttu-id="b5a08-160">När du behöver göra något med den virtuella datorn måste du hämta en instans av den.</span><span class="sxs-lookup"><span data-stu-id="b5a08-160">When you need to do anything with the VM, you need to get an instance of it.</span></span> <span data-ttu-id="b5a08-161">Lägg till den här koden try-block av main-metoden:</span><span class="sxs-lookup"><span data-stu-id="b5a08-161">Add this code to the try block of the main method:</span></span>

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a><span data-ttu-id="b5a08-162">Hämta information om den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-162">Get information about the VM</span></span>

<span data-ttu-id="b5a08-163">Lägg till den här koden try-block i main-metoden för att få information om den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="b5a08-163">To get information about the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();   
```

### <a name="stop-the-vm"></a><span data-ttu-id="b5a08-164">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-164">Stop the VM</span></span>

<span data-ttu-id="b5a08-165">Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätter att debiteras för den eller stoppa en virtuell dator och frigör den.</span><span class="sxs-lookup"><span data-stu-id="b5a08-165">You can stop a virtual machine and keep all its settings, but continue to be charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="b5a08-166">När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.</span><span class="sxs-lookup"><span data-stu-id="b5a08-166">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="b5a08-167">Lägg till den här koden try-block i main-metoden för att stoppa den virtuella datorn utan att det har frigjorts den:</span><span class="sxs-lookup"><span data-stu-id="b5a08-167">To stop the virtual machine without deallocating it, add this code to the try block in the main method:</span></span>

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

<span data-ttu-id="b5a08-168">Om du vill ta bort den virtuella datorn ändra avstängningsläge anrop till den här koden:</span><span class="sxs-lookup"><span data-stu-id="b5a08-168">If you want to deallocate the virtual machine, change the PowerOff call to this code:</span></span>

```java
vm.deallocate();
```

### <a name="start-the-vm"></a><span data-ttu-id="b5a08-169">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-169">Start the VM</span></span>

<span data-ttu-id="b5a08-170">Lägg till den här koden try-block i main-metoden för att starta den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="b5a08-170">To start the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a><span data-ttu-id="b5a08-171">Ändra storlek på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-171">Resize the VM</span></span>

<span data-ttu-id="b5a08-172">Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b5a08-172">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="b5a08-173">Mer information finns i [VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="b5a08-173">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="b5a08-174">Lägg till den här koden try-block i main-metoden om du vill ändra storleken på den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="b5a08-174">To change size of the virtual machine, add this code to the try block in the main method:</span></span>

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a><span data-ttu-id="b5a08-175">Lägg till en datadisk till den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b5a08-175">Add a data disk to the VM</span></span>

<span data-ttu-id="b5a08-176">Lägg till den här koden try-block i main-metoden för att lägga till en datadisk till den virtuella datorn som är 2 GB i storlek, har ett LUN 0 och en cachelagring typ av ReadWrite:</span><span class="sxs-lookup"><span data-stu-id="b5a08-176">To add a data disk to the virtual machine that is 2 GB in size, has a LUN of 0, and a caching type of ReadWrite, add this code to the try block in the main method:</span></span>

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a><span data-ttu-id="b5a08-177">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="b5a08-177">Delete resources</span></span>

<span data-ttu-id="b5a08-178">Eftersom du debiteras för de resurser som används i Azure, men det är alltid bra att ta bort resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="b5a08-178">Because you are charged for resources used in Azure, it is always good practice to delete resources that are no longer needed.</span></span> <span data-ttu-id="b5a08-179">Om du vill ta bort de virtuella datorerna och alla stödresurser är allt du behöver göra resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-179">If you want to delete the virtual machines and all the supporting resources, all you have to do is delete the resource group.</span></span>

1. <span data-ttu-id="b5a08-180">Lägg till den här koden try-block i main-metoden för att ta bort resursgruppen:</span><span class="sxs-lookup"><span data-stu-id="b5a08-180">To delete the resource group, add this code to the try block in the main method:</span></span>
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. <span data-ttu-id="b5a08-181">Spara filen App.java.</span><span class="sxs-lookup"><span data-stu-id="b5a08-181">Save the App.java file.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="b5a08-182">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="b5a08-182">Run the application</span></span>

<span data-ttu-id="b5a08-183">Det bör ta ungefär fem minuter för den här konsolen programmet helt från början till slut.</span><span class="sxs-lookup"><span data-stu-id="b5a08-183">It should take about five minutes for this console application to run completely from start to finish.</span></span>

1. <span data-ttu-id="b5a08-184">Använd följande Maven-kommando för att köra programmet:</span><span class="sxs-lookup"><span data-stu-id="b5a08-184">To run the application, use this Maven command:</span></span>

    ```
    mvn compile exec:java
    ```

2. <span data-ttu-id="b5a08-185">Innan du trycker på **ange** om du vill börja ta bort resurser, du kan ta några minuter för att verifiera att skapa resurser i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b5a08-185">Before you press **Enter** to start deleting resources, you could take a few minutes to verify the creation of the resources in the Azure portal.</span></span> <span data-ttu-id="b5a08-186">Klicka på Distributionsstatus för att visa information om hur du distribuerar.</span><span class="sxs-lookup"><span data-stu-id="b5a08-186">Click the deployment status to see information about the deployment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="b5a08-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b5a08-187">Next steps</span></span>
* <span data-ttu-id="b5a08-188">Lär dig mer om hur du använder den [Azure-bibliotek för Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span><span class="sxs-lookup"><span data-stu-id="b5a08-188">Learn more about using the [Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span></span>

