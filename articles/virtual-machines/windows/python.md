---
title: aaaCreate och hantera en virtuell Windows-dator i Azure med Python | Microsoft Docs
description: "Lär dig toouse Python toocreate och hantera en virtuell Windows-dator i Azure."
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
ms.date: 06/22/2017
ms.author: davidmu
ms.openlocfilehash: c5553e4e7361e6b9a7183cd935be382f967160cb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a>Skapa och hantera virtuella Windows-datorer i Azure med Python

En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser. Den här artikeln beskriver hur du skapar, hantera och ta bort VM-resurser med hjälp av Python. Lär dig att:

> [!div class="checklist"]
> * Skapa ett Visual Studio-projekt
> * Installera paket
> * Skapa autentiseringsuppgifter
> * Skapa resurser
> * Utföra administrativa uppgifter
> * Ta bort resurser
> * Kör programmet hello

Det tar cirka 20 minuter toodo dessa steg.

## <a name="create-a-visual-studio-project"></a>Skapa ett Visual Studio-projekt

1. Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Välj **utveckling av Python** på hello arbetsbelastningar sidan och klicka sedan på **installera**. I hello sammanfattning, kan du se att **Python 3 64-bitars (3.6.0)** väljs automatiskt för dig. Om du redan har installerat Visual Studio kan du lägga till hello Python arbetsbelastning med hello Visual Studio starta.
2. Efter installation och start av Visual Studio klickar du på **filen** > **ny** > **projekt**.
3. Klicka på **mallar** > **Python** > **Python programmet**, ange *myPythonProject* för hello namn hello Project välja hello platsen för hello projektet och klicka sedan på **OK**.

## <a name="install-packages"></a>Installera paket

1. I Solution Explorer under *myPythonProject*, högerklicka på **Python-miljöer**, och välj sedan **Lägg till virtuell miljö**.
2. Acceptera hello standardnamnet på hello-skärmen Lägg till virtuell miljö *env*, se till att *Python 3,6 (64-bitars)* har valts för hello bastolk och klicka sedan på **skapa**.
3. Högerklicka på hello *env* miljö som du har skapat, klicka på **Install Python Package**, ange *azure* i hello sökrutan och tryck sedan på RETUR.

Du bör se i hello utdata windows hello azure-paket har installerats. 

## <a name="create-credentials"></a>Skapa autentiseringsuppgifter

Innan du startar det här steget, se till att du har en [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Du bör anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i ett senare steg.

1. Öppna *myPythonProject.py* fil som har skapats och Lägg sedan till den här koden tooenable toorun ditt program:

    ```python
    if __name__ == "__main__":
    ```

2. tooimport hello kod som krävs, lägger du till dessa instruktioner toohello överkant hello .py-fil:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. Nästa hello .py-fil, lägga till variabler när hello importuttryck toospecify vanliga värden används i hello kod:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    Ersätt **prenumerations-id** med prenumerations-ID.

4. toocreate hello Active Directory-autentiseringsuppgifter som du behöver toomake begäranden, lägger till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    Ersätt **program-id**, **autentiseringsnyckel**, och **klient-id** med hello-värden som du tidigare har samlat in när du skapade din Azure Active Directory tjänstens huvudnamn.

5. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Skapa resurser
 
### <a name="initialize-management-clients"></a>Initiera av hanteringsklienter

Av hanteringsklienter är nödvändiga toocreate och hantera resurser med hjälp av hello Python SDK i Azure. toocreate hello hantering av klienter, Lägg till denna kod under hello **om** instruktionen sedan slutet av hello .py-fil:

```python
resource_group_client = ResourceManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
network_client = NetworkManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
compute_client = ComputeManagementClient(
    credentials, 
    SUBSCRIPTION_ID
)
```

### <a name="create-hello-vm-and-supporting-resources"></a>Skapa hello VM och stöd för resurser

Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).

1. toocreate en resursgrupp, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare för dig toomaintain hello virtuella datorer som används av ditt program.

1. toocreate tillgänglighet ange, Lägg till den här funktionen efter hello variabler i hello .py-fil:
   
    ```python
    def create_availability_set(compute_client):
        avset_params = {
            'location': LOCATION,
            'sku': { 'name': 'Aligned' },
            'platform_fault_domain_count': 3
        }
        availability_set_result = compute_client.availability_sets.create_or_update(
            GROUP_NAME,
            'myAVSet',
            avset_params
        )
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) är nödvändiga toocommunicate med hello virtuell dator.

1. toocreate offentlig IP-adress för hello virtuell dator, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def create_public_ip_address(network_client):
        public_ip_addess_params = {
            'location': LOCATION,
            'public_ip_allocation_method': 'Dynamic'
        }
        creation_result = network_client.public_ip_addresses.create_or_update(
            GROUP_NAME,
            'myIPAddress',
            public_ip_addess_params
        )

        return creation_result.result()
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).

1. toocreate ett virtuellt nätverk, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def create_vnet(network_client):
        vnet_params = {
            'location': LOCATION,
            'address_space': {
                'address_prefixes': ['10.0.0.0/16']
            }
        }
        creation_result = network_client.virtual_networks.create_or_update(
            GROUP_NAME,
            'myVNet',
            vnet_params
        )
        return creation_result.result()
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. tooadd en toohello virtuella undernätverk, Lägg till den här funktionen efter hello variabler i hello .py-fil:
    
    ```python
    def create_subnet(network_client):
        subnet_params = {
            'address_prefix': '10.0.0.0/24'
        }
        creation_result = network_client.subnets.create_or_update(
            GROUP_NAME,
            'myVNet',
            'mySubnet',
            subnet_params
        )

        return creation_result.result()
    ```
        
4. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

En virtuell dator måste en network interface toocommunicate hello virtuella nätverket.

1. toocreate nätverksgränssnitt, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def create_nic(network_client):
        subnet_info = network_client.subnets.get(
            GROUP_NAME, 
            'myVNet', 
            'mySubnet'
        )
        publicIPAddress = network_client.public_ip_addresses.get(
            GROUP_NAME,
            'myIPAddress'
        )
        nic_params = {
            'location': LOCATION,
            'ip_configurations': [{
                'name': 'myIPConfig',
                'public_ip_address': publicIPAddress,
                'subnet': {
                    'id': subnet_info.id
                }
            }]
        }
        creation_result = network_client.network_interfaces.create_or_update(
            GROUP_NAME,
            'myNic',
            nic_params
        )

        return creation_result.result()
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

Nu när du har skapat alla hello stöder resurser kan du skapa en virtuell dator.

1. toocreate Hej virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:
   
    ```python
    def create_vm(network_client, compute_client):  
        nic = network_client.network_interfaces.get(
            GROUP_NAME, 
            'myNic'
        )
        avset = compute_client.availability_sets.get(
            GROUP_NAME,
            'myAVSet'
        )
        vm_parameters = {
            'location': LOCATION,
            'os_profile': {
                'computer_name': VM_NAME,
                'admin_username': 'azureuser',
                'admin_password': 'Azure12345678'
            },
            'hardware_profile': {
                'vm_size': 'Standard_DS1'
            },
            'storage_profile': {
                'image_reference': {
                    'publisher': 'MicrosoftWindowsServer',
                    'offer': 'WindowsServer',
                    'sku': '2012-R2-Datacenter',
                    'version': 'latest'
                }
            },
            'network_profile': {
                'network_interfaces': [{
                    'id': nic.id
                }]
            },
            'availability_set': {
                'id': avset.id
            }
        }
        creation_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm_parameters
        )
    
        return creation_result.result()
    ```

    > [!NOTE]
    > Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server för hello. toolearn mer information om hur du väljer andra bilder, se [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a>Utföra administrativa uppgifter

Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator. Dessutom kan du toocreate kod tooautomate repetitiva och komplicerade uppgifter.

### <a name="get-information-about-hello-vm"></a>Hämta information om hello VM

1. tooget information om hello virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def get_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME, expand='instanceView')
        print("hardwareProfile")
        print("   vmSize: ", vm.hardware_profile.vm_size)
        print("\nstorageProfile")
        print("  imageReference")
        print("    publisher: ", vm.storage_profile.image_reference.publisher)
        print("    offer: ", vm.storage_profile.image_reference.offer)
        print("    sku: ", vm.storage_profile.image_reference.sku)
        print("    version: ", vm.storage_profile.image_reference.version)
        print("  osDisk")
        print("    osType: ", vm.storage_profile.os_disk.os_type.value)
        print("    name: ", vm.storage_profile.os_disk.name)
        print("    createOption: ", vm.storage_profile.os_disk.create_option.value)
        print("    caching: ", vm.storage_profile.os_disk.caching.value)
        print("\nosProfile")
        print("  computerName: ", vm.os_profile.computer_name)
        print("  adminUsername: ", vm.os_profile.admin_username)
        print("  provisionVMAgent: {0}".format(vm.os_profile.windows_configuration.provision_vm_agent))
        print("  enableAutomaticUpdates: {0}".format(vm.os_profile.windows_configuration.enable_automatic_updates))
        print("\nnetworkProfile")
        for nic in vm.network_profile.network_interfaces:
            print("  networkInterface id: ", nic.id)
        print("\nvmAgent")
        print("  vmAgentVersion", vm.instance_view.vm_agent.vm_agent_version)
        print("    statuses")
        for stat in vm_result.instance_view.vm_agent.statuses:
            print("    code: ", stat.code)
            print("    displayStatus: ", stat.display_status)
            print("    message: ", stat.message)
            print("    time: ", stat.time)
        print("\ndisks");
        for disk in vm.instance_view.disks:
            print("  name: ", disk.name)
            print("  statuses")
            for stat in disk.statuses:
                print("    code: ", stat.code)
                print("    displayStatus: ", stat.display_status)
                print("    time: ", stat.time)
        print("\nVM general status")
        print("  provisioningStatus: ", vm.provisioning_state)
        print("  id: ", vm.id)
        print("  name: ", vm.name)
        print("  type: ", vm.type)
        print("  location: ", vm.location)
        print("\nVM instance status")
        for stat in vm.instance_view.statuses:
            print("  code: ", stat.code)
            print("  displayStatus: ", stat.display_status)
    ```
2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a>Stoppa hello VM

Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätta toobe debiteras för den eller stoppa en virtuell dator och frigör den. När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.

1. toostop hello virtuell dator utan att det har frigjorts, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Om du vill toodeallocate hello virtuella datorn, ändra hello power_off anropet toothis koden:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a>Starta hello VM

1. toostart Hej virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a>Ändra storlek på hello VM

Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn. Mer information finns i [VM-storlekar](sizes.md).

1. toochange hello storleken på hello virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:

    ```python
    def update_vm(compute_client):
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        vm.hardware_profile.vm_size = 'Standard_DS3'
        update_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME, 
            VM_NAME, 
            vm
        )

    return update_result.result()
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a>Lägg till en data disk toohello VM

Virtuella datorer kan ha en eller flera [datadiskar](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som lagras som virtuella hårddiskar.

1. tooadd en data disk toohello virtuell dator, Lägg till den här funktionen efter hello variabler i hello .py-fil: 

    ```python
    def add_datadisk(compute_client):
        disk_creation = compute_client.disks.create_or_update(
            GROUP_NAME,
            'myDataDisk1',
            {
                'location': LOCATION,
                'disk_size_gb': 1,
                'creation_data': {
                    'create_option': DiskCreateOption.empty
                }
            }
        )
        data_disk = disk_creation.result()
        vm = compute_client.virtual_machines.get(GROUP_NAME, VM_NAME)
        add_result = vm.storage_profile.data_disks.append({
            'lun': 1,
            'name': 'myDataDisk1',
            'create_option': DiskCreateOption.attach,
            'managed_disk': {
                'id': data_disk.id
            }
        })
        add_result = compute_client.virtual_machines.create_or_update(
            GROUP_NAME,
            VM_NAME,
            vm)

        return add_result.result()
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a>Ta bort resurser

Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé toodelete resurser som inte längre behövs. Om du vill toodelete hello virtuella datorer och alla hello stöder resurser, alla har toodo är delete hello resursgruppen.

1. Lägg till den här funktionen efter hello variabler i hello .py-fil toodelete hello resursgruppen och alla resurser:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. Spara *myPythonProject.py*.

## <a name="run-hello-application"></a>Kör programmet hello

1. toorun hello-konsolprogram klickar du på **starta** i Visual Studio.

2. Tryck på **RETUR** när returneras hello status för varje resurs. I hello statusinformation, bör du se en **lyckades** Etableringsstatus. När hello virtuell dator skapas har du hello möjlighet toodelete alla hello-resurser som du skapar. Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify skapas i hello Azure-portalen. Om du har hello Azure portal öppna kanske toorefresh hello bladet toosee nya resurser.  

    Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish. Det kan ta flera minuter efter hello program har slutförts innan alla hello-resurser och hello resursgruppen tas bort.


## <a name="next-steps"></a>Nästa steg

- Om det fanns problem med hello distribution, nästa steg är toolook på [felsöka distributionen av resursgrupper med Azure-portalen](../../resource-manager-troubleshoot-deployments-portal.md)
- Mer information om hello [Azure Python-bibliotek](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)

