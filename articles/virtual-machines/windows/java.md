---
title: "aaaCreate och hantera Azure virtuella datorer med hjälp av Java | Microsoft Docs"
description: "Använd Java och Azure Resource Manager toodeploy en virtuell dator och alla dess stödfiler resurser."
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
ms.openlocfilehash: 31ac8d59f92ecff887e64906940933dd6fd50815
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a><span data-ttu-id="d2edf-103">Skapa och hantera virtuella Windows-datorer i Azure som använder Java</span><span class="sxs-lookup"><span data-stu-id="d2edf-103">Create and manage Windows VMs in Azure using Java</span></span>

<span data-ttu-id="d2edf-104">En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="d2edf-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="d2edf-105">Den här artikeln beskriver hur du skapar, hantera och ta bort VM-resurser med hjälp av Java.</span><span class="sxs-lookup"><span data-stu-id="d2edf-105">This article covers creating, managing, and deleting VM resources using Java.</span></span> <span data-ttu-id="d2edf-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="d2edf-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d2edf-107">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="d2edf-107">Create a Maven project</span></span>
> * <span data-ttu-id="d2edf-108">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="d2edf-108">Add dependencies</span></span>
> * <span data-ttu-id="d2edf-109">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d2edf-109">Create credentials</span></span>
> * <span data-ttu-id="d2edf-110">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="d2edf-110">Create resources</span></span>
> * <span data-ttu-id="d2edf-111">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="d2edf-111">Perform management tasks</span></span>
> * <span data-ttu-id="d2edf-112">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="d2edf-112">Delete resources</span></span>
> * <span data-ttu-id="d2edf-113">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="d2edf-113">Run hello application</span></span>

<span data-ttu-id="d2edf-114">Det tar cirka 20 minuter toodo dessa steg.</span><span class="sxs-lookup"><span data-stu-id="d2edf-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-maven-project"></a><span data-ttu-id="d2edf-115">Skapa ett Maven-projekt</span><span class="sxs-lookup"><span data-stu-id="d2edf-115">Create a Maven project</span></span>

1. <span data-ttu-id="d2edf-116">Om du inte redan har gjort det installerar [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="d2edf-116">If you haven't already done so, install [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).</span></span>
2. <span data-ttu-id="d2edf-117">Installera [Maven](http://maven.apache.org/download.cgi).</span><span class="sxs-lookup"><span data-stu-id="d2edf-117">Install [Maven](http://maven.apache.org/download.cgi).</span></span>
3. <span data-ttu-id="d2edf-118">Skapa en ny mapp och hello projektet:</span><span class="sxs-lookup"><span data-stu-id="d2edf-118">Create a new folder and hello project:</span></span>
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a><span data-ttu-id="d2edf-119">Lägga till beroenden</span><span class="sxs-lookup"><span data-stu-id="d2edf-119">Add dependencies</span></span>

1. <span data-ttu-id="d2edf-120">Under hello `testAzureApp` mapp, öppna hello `pom.xml` och Lägg till hello versionskonfiguration för&lt;projekt&gt; tooenable hello bygga i ditt program:</span><span class="sxs-lookup"><span data-stu-id="d2edf-120">Under hello `testAzureApp` folder, open hello `pom.xml` file and add hello build configuration too&lt;project&gt; tooenable hello building of your application:</span></span>

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

2. <span data-ttu-id="d2edf-121">Lägg till hello beroenden som är nödvändiga tooaccess hello Azure Java SDK.</span><span class="sxs-lookup"><span data-stu-id="d2edf-121">Add hello dependencies that are needed tooaccess hello Azure Java SDK.</span></span>

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

3. <span data-ttu-id="d2edf-122">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-122">Save hello file.</span></span>

## <a name="create-credentials"></a><span data-ttu-id="d2edf-123">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d2edf-123">Create credentials</span></span>

<span data-ttu-id="d2edf-124">Innan du startar det här steget, se till att du har åtkomst tooan [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d2edf-124">Before you start this step, make sure that you have access tooan [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="d2edf-125">Du bör anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="d2edf-125">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

### <a name="create-hello-authorization-file"></a><span data-ttu-id="d2edf-126">Skapa hello auktoriseringsfilen</span><span class="sxs-lookup"><span data-stu-id="d2edf-126">Create hello authorization file</span></span>

1. <span data-ttu-id="d2edf-127">Skapa en fil med namnet `azureauth.properties` och lägga till dessa egenskaper tooit:</span><span class="sxs-lookup"><span data-stu-id="d2edf-127">Create a file named `azureauth.properties` and add these properties tooit:</span></span>

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

    <span data-ttu-id="d2edf-128">Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med hello Active Directory-program identifierare,  **&lt;autentiseringsnyckel&gt;**  med hello programmet nyckel och  **&lt;klient-id&gt;**  med hello-klient identifierare.</span><span class="sxs-lookup"><span data-stu-id="d2edf-128">Replace **&lt;subscription-id&gt;** with your subscription identifier, **&lt;application-id&gt;** with hello Active Directory application identifier, **&lt;authentication-key&gt;** with hello application key, and **&lt;tenant-id&gt;** with hello tenant identifier.</span></span>

2. <span data-ttu-id="d2edf-129">Spara hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-129">Save hello file.</span></span>
3. <span data-ttu-id="d2edf-130">Ange miljövariabeln AZURE_AUTH_LOCATION i gränssnittet med hello fullständig sökväg toohello autentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-130">Set an environment variable named AZURE_AUTH_LOCATION in your shell with hello full path toohello authentication file.</span></span>

### <a name="create-hello-management-client"></a><span data-ttu-id="d2edf-131">Skapa hello management-klienten</span><span class="sxs-lookup"><span data-stu-id="d2edf-131">Create hello management client</span></span>

1. <span data-ttu-id="d2edf-132">Öppna hello `App.java` filen `src\main\java\com\fabrikam` och kontrollera att det här paketet instruktionen är överst hello:</span><span class="sxs-lookup"><span data-stu-id="d2edf-132">Open hello `App.java` file under `src\main\java\com\fabrikam` and make sure this package statement is at hello top:</span></span>

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. <span data-ttu-id="d2edf-133">Lägg till dessa under hello paketet instruktionen importera instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d2edf-133">Under hello package statement, add these import statements:</span></span>
   
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

2. <span data-ttu-id="d2edf-134">toocreate hello Active Directory-autentiseringsuppgifter som du behöver toomake begäranden, lägger du till den här koden toohello main-metoden för hello App klass:</span><span class="sxs-lookup"><span data-stu-id="d2edf-134">toocreate hello Active Directory credentials that you need toomake requests, add this code toohello main method of hello App class:</span></span>
   
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

## <a name="create-resources"></a><span data-ttu-id="d2edf-135">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="d2edf-135">Create resources</span></span>

### <a name="create-hello-resource-group"></a><span data-ttu-id="d2edf-136">Skapa hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="d2edf-136">Create hello resource group</span></span>

<span data-ttu-id="d2edf-137">Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2edf-137">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

<span data-ttu-id="d2edf-138">toospecify som värden för hello programmet och skapa hello resursgrupp, lägga till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-138">toospecify values for hello application and create hello resource group, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a><span data-ttu-id="d2edf-139">Skapa hello tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="d2edf-139">Create hello availability set</span></span>

<span data-ttu-id="d2edf-140">[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare för dig toomaintain hello virtuella datorer som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="d2edf-140">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

<span data-ttu-id="d2edf-141">toocreate hello tillgänglighet ange, lägga till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-141">toocreate hello availability set, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a><span data-ttu-id="d2edf-142">Skapa hello offentlig IP-adress</span><span class="sxs-lookup"><span data-stu-id="d2edf-142">Create hello public IP address</span></span>

<span data-ttu-id="d2edf-143">En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) är nödvändiga toocommunicate med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2edf-143">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

<span data-ttu-id="d2edf-144">toocreate hello offentliga IP-adressen för hello virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-144">toocreate hello public IP address for hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a><span data-ttu-id="d2edf-145">Skapa hello virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="d2edf-145">Create hello virtual network</span></span>

<span data-ttu-id="d2edf-146">En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="d2edf-146">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

<span data-ttu-id="d2edf-147">toocreate ett undernät och virtuella nätverk, kan du lägga till den här koden toohello try-block på hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-147">toocreate a subnet and a virtual network, add this code toohello try block in hello main method:</span></span>

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

### <a name="create-hello-network-interface"></a><span data-ttu-id="d2edf-148">Skapa hello nätverksgränssnittet</span><span class="sxs-lookup"><span data-stu-id="d2edf-148">Create hello network interface</span></span>

<span data-ttu-id="d2edf-149">En virtuell dator måste en network interface toocommunicate hello virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="d2edf-149">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

<span data-ttu-id="d2edf-150">toocreate nätverksgränssnitt, lägga till den här koden toohello try-block på hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-150">toocreate a network interface, add this code toohello try block in hello main method:</span></span>

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

### <a name="create-hello-virtual-machine"></a><span data-ttu-id="d2edf-151">Skapa hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d2edf-151">Create hello virtual machine</span></span>

<span data-ttu-id="d2edf-152">Nu när du har skapat alla hello stöder resurser kan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2edf-152">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

<span data-ttu-id="d2edf-153">toocreate Hej virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-153">toocreate hello virtual machine, add this code toohello try block in hello main method:</span></span>

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
System.out.println("Press enter tooget information about hello VM...");
input.nextLine();
```

> [!NOTE]
> <span data-ttu-id="d2edf-154">Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server för hello.</span><span class="sxs-lookup"><span data-stu-id="d2edf-154">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="d2edf-155">toolearn mer information om hur du väljer andra bilder, se [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="d2edf-155">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
> 
>

<span data-ttu-id="d2edf-156">Om du vill toouse en befintlig disk i stället för en marketplace-avbildning, använder du den här koden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-156">If you want toouse an existing disk instead of a marketplace image, use this code:</span></span> 

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

## <a name="perform-management-tasks"></a><span data-ttu-id="d2edf-157">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="d2edf-157">Perform management tasks</span></span>

<span data-ttu-id="d2edf-158">Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d2edf-158">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="d2edf-159">Dessutom kan du toocreate kod tooautomate repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="d2edf-159">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

<span data-ttu-id="d2edf-160">När du behöver toodo något med hello VM, behöver du tooget en instans av den.</span><span class="sxs-lookup"><span data-stu-id="d2edf-160">When you need toodo anything with hello VM, you need tooget an instance of it.</span></span> <span data-ttu-id="d2edf-161">Lägg till den här koden toohello try-block av hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-161">Add this code toohello try block of hello main method:</span></span>

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="d2edf-162">Hämta information om hello VM</span><span class="sxs-lookup"><span data-stu-id="d2edf-162">Get information about hello VM</span></span>

<span data-ttu-id="d2edf-163">tooget information om hello virtuella datorn, Lägg till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-163">tooget information about hello virtual machine, add this code toohello try block in hello main method:</span></span>

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
System.out.println("Press enter toocontinue...");
input.nextLine();   
```

### <a name="stop-hello-vm"></a><span data-ttu-id="d2edf-164">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="d2edf-164">Stop hello VM</span></span>

<span data-ttu-id="d2edf-165">Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätta toobe debiteras för den eller stoppa en virtuell dator och frigör den.</span><span class="sxs-lookup"><span data-stu-id="d2edf-165">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="d2edf-166">När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.</span><span class="sxs-lookup"><span data-stu-id="d2edf-166">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

<span data-ttu-id="d2edf-167">toostop hello virtuell dator utan att det har frigjorts, lägga till den här koden toohello try-block på hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-167">toostop hello virtual machine without deallocating it, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

<span data-ttu-id="d2edf-168">Om du vill toodeallocate hello virtuella datorn, ändra hello avstängningsläge anropet toothis koden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-168">If you want toodeallocate hello virtual machine, change hello PowerOff call toothis code:</span></span>

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a><span data-ttu-id="d2edf-169">Starta hello VM</span><span class="sxs-lookup"><span data-stu-id="d2edf-169">Start hello VM</span></span>

<span data-ttu-id="d2edf-170">toostart Hej virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-170">toostart hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a><span data-ttu-id="d2edf-171">Ändra storlek på hello VM</span><span class="sxs-lookup"><span data-stu-id="d2edf-171">Resize hello VM</span></span>

<span data-ttu-id="d2edf-172">Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d2edf-172">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="d2edf-173">Mer information finns i [VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="d2edf-173">For more information, see [VM sizes](sizes.md).</span></span>  

<span data-ttu-id="d2edf-174">toochange storleken på hello virtuella datorn, Lägg till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-174">toochange size of hello virtual machine, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="d2edf-175">Lägg till en data disk toohello VM</span><span class="sxs-lookup"><span data-stu-id="d2edf-175">Add a data disk toohello VM</span></span>

<span data-ttu-id="d2edf-176">tooadd en data disk toohello virtuell dator som är 2 GB i storlek, har ett LUN 0 och en cachelagring typ av ReadWrite, lägga till den här koden toohello try-block på hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-176">tooadd a data disk toohello virtual machine that is 2 GB in size, has a LUN of 0, and a caching type of ReadWrite, add this code toohello try block in hello main method:</span></span>

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a><span data-ttu-id="d2edf-177">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="d2edf-177">Delete resources</span></span>

<span data-ttu-id="d2edf-178">Eftersom debiteras du för resurser som används i Azure, men det är alltid bra toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="d2edf-178">Because you are charged for resources used in Azure, it is always good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="d2edf-179">Om du vill toodelete hello virtuella datorer och alla hello stöder resurser, alla har toodo är delete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-179">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="d2edf-180">toodelete hello resurs och lägga till den här koden toohello try-block i hello main-metoden:</span><span class="sxs-lookup"><span data-stu-id="d2edf-180">toodelete hello resource group, add this code toohello try block in hello main method:</span></span>
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. <span data-ttu-id="d2edf-181">Spara hello App.java filen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-181">Save hello App.java file.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="d2edf-182">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="d2edf-182">Run hello application</span></span>

<span data-ttu-id="d2edf-183">Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish.</span><span class="sxs-lookup"><span data-stu-id="d2edf-183">It should take about five minutes for this console application toorun completely from start toofinish.</span></span>

1. <span data-ttu-id="d2edf-184">toorun Hej program, använder du följande Maven-kommando:</span><span class="sxs-lookup"><span data-stu-id="d2edf-184">toorun hello application, use this Maven command:</span></span>

    ```
    mvn compile exec:java
    ```

2. <span data-ttu-id="d2edf-185">Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify hello skapandet av hello resurser i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d2edf-185">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify hello creation of hello resources in hello Azure portal.</span></span> <span data-ttu-id="d2edf-186">Klicka på hello toosee information om Distributionsstatus om hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="d2edf-186">Click hello deployment status toosee information about hello deployment.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d2edf-187">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2edf-187">Next steps</span></span>
* <span data-ttu-id="d2edf-188">Lär dig mer om hur du använder hello [Azure-bibliotek för Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span><span class="sxs-lookup"><span data-stu-id="d2edf-188">Learn more about using hello [Azure libraries for Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).</span></span>

