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
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Skapa och hantera virtuella Windows-datorer i Azure som använder Java

En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser. Den här artikeln beskriver hur du skapar, hantera och ta bort VM-resurser med hjälp av Java. Lär dig att:

> [!div class="checklist"]
> * Skapa ett Maven-projekt
> * Lägga till beroenden
> * Skapa autentiseringsuppgifter
> * Skapa resurser
> * Utföra administrativa uppgifter
> * Ta bort resurser
> * Kör programmet hello

Det tar cirka 20 minuter toodo dessa steg.

## <a name="create-a-maven-project"></a>Skapa ett Maven-projekt

1. Om du inte redan har gjort det installerar [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Installera [Maven](http://maven.apache.org/download.cgi).
3. Skapa en ny mapp och hello projektet:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Lägga till beroenden

1. Under hello `testAzureApp` mapp, öppna hello `pom.xml` och Lägg till hello versionskonfiguration för&lt;projekt&gt; tooenable hello bygga i ditt program:

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

2. Lägg till hello beroenden som är nödvändiga tooaccess hello Azure Java SDK.

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

3. Spara hello-filen.

## <a name="create-credentials"></a>Skapa autentiseringsuppgifter

Innan du startar det här steget, se till att du har åtkomst tooan [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Du bör anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i ett senare steg.

### <a name="create-hello-authorization-file"></a>Skapa hello auktoriseringsfilen

1. Skapa en fil med namnet `azureauth.properties` och lägga till dessa egenskaper tooit:

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

    Ersätt  **&lt;prenumerations-id&gt;**  med prenumerations-ID  **&lt;program-id&gt;**  med hello Active Directory-program identifierare,  **&lt;autentiseringsnyckel&gt;**  med hello programmet nyckel och  **&lt;klient-id&gt;**  med hello-klient identifierare.

2. Spara hello-filen.
3. Ange miljövariabeln AZURE_AUTH_LOCATION i gränssnittet med hello fullständig sökväg toohello autentiseringsfilen.

### <a name="create-hello-management-client"></a>Skapa hello management-klienten

1. Öppna hello `App.java` filen `src\main\java\com\fabrikam` och kontrollera att det här paketet instruktionen är överst hello:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Lägg till dessa under hello paketet instruktionen importera instruktioner:
   
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

2. toocreate hello Active Directory-autentiseringsuppgifter som du behöver toomake begäranden, lägger du till den här koden toohello main-metoden för hello App klass:
   
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

## <a name="create-resources"></a>Skapa resurser

### <a name="create-hello-resource-group"></a>Skapa hello resursgrupp

Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).

toospecify som värden för hello programmet och skapa hello resursgrupp, lägga till den här koden toohello try-block i hello main-metoden:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-hello-availability-set"></a>Skapa hello tillgänglighetsuppsättning

[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare för dig toomaintain hello virtuella datorer som används av ditt program.

toocreate hello tillgänglighet ange, lägga till den här koden toohello try-block i hello main-metoden:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-hello-public-ip-address"></a>Skapa hello offentlig IP-adress

En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) är nödvändiga toocommunicate med hello virtuell dator.

toocreate hello offentliga IP-adressen för hello virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-hello-virtual-network"></a>Skapa hello virtuellt nätverk

En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).

toocreate ett undernät och virtuella nätverk, kan du lägga till den här koden toohello try-block på hello main-metoden:

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

### <a name="create-hello-network-interface"></a>Skapa hello nätverksgränssnittet

En virtuell dator måste en network interface toocommunicate hello virtuella nätverket.

toocreate nätverksgränssnitt, lägga till den här koden toohello try-block på hello main-metoden:

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

### <a name="create-hello-virtual-machine"></a>Skapa hello virtuell dator

Nu när du har skapat alla hello stöder resurser kan du skapa en virtuell dator.

toocreate Hej virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:

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
> Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server för hello. toolearn mer information om hur du väljer andra bilder, se [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Om du vill toouse en befintlig disk i stället för en marketplace-avbildning, använder du den här koden: 

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

## <a name="perform-management-tasks"></a>Utföra administrativa uppgifter

Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator. Dessutom kan du toocreate kod tooautomate repetitiva och komplicerade uppgifter.

När du behöver toodo något med hello VM, behöver du tooget en instans av den. Lägg till den här koden toohello try-block av hello main-metoden:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-hello-vm"></a>Hämta information om hello VM

tooget information om hello virtuella datorn, Lägg till den här koden toohello try-block i hello main-metoden:

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

### <a name="stop-hello-vm"></a>Stoppa hello VM

Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätta toobe debiteras för den eller stoppa en virtuell dator och frigör den. När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.

toostop hello virtuell dator utan att det har frigjorts, lägga till den här koden toohello try-block på hello main-metoden:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

Om du vill toodeallocate hello virtuella datorn, ändra hello avstängningsläge anropet toothis koden:

```java
vm.deallocate();
```

### <a name="start-hello-vm"></a>Starta hello VM

toostart Hej virtuell dator, lägger du till den här koden toohello try-block i hello main-metoden:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="resize-hello-vm"></a>Ändra storlek på hello VM

Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn. Mer information finns i [VM-storlekar](sizes.md).  

toochange storleken på hello virtuella datorn, Lägg till den här koden toohello try-block i hello main-metoden:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter toocontinue...");
input.nextLine();
```

### <a name="add-a-data-disk-toohello-vm"></a>Lägg till en data disk toohello VM

tooadd en data disk toohello virtuell dator som är 2 GB i storlek, har ett LUN 0 och en cachelagring typ av ReadWrite, lägga till den här koden toohello try-block på hello main-metoden:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter toodelete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Ta bort resurser

Eftersom debiteras du för resurser som används i Azure, men det är alltid bra toodelete resurser som inte längre behövs. Om du vill toodelete hello virtuella datorer och alla hello stöder resurser, alla har toodo är delete hello resursgruppen.

1. toodelete hello resurs och lägga till den här koden toohello try-block i hello main-metoden:
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. Spara hello App.java filen.

## <a name="run-hello-application"></a>Kör programmet hello

Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish.

1. toorun Hej program, använder du följande Maven-kommando:

    ```
    mvn compile exec:java
    ```

2. Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify hello skapandet av hello resurser i hello Azure-portalen. Klicka på hello toosee information om Distributionsstatus om hello-distribution.


## <a name="next-steps"></a>Nästa steg
* Lär dig mer om hur du använder hello [Azure-bibliotek för Java](https://docs.microsoft.com/en-us/java/azure/java-sdk-azure-overview).

