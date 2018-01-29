---
title: Skapa och hantera en virtuell Windows-dator i Azure med Python | Microsoft Docs
description: "Lär dig använda Python att skapa och hantera en virtuell Windows-dator i Azure."
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
ms.openlocfilehash: bb777d41570d7b1dc97402d532519488912948e3
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
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
> * Köra programmet

Det tar ungefär 20 minuter för att utföra de här stegen.

## <a name="create-a-visual-studio-project"></a>Skapa ett Visual Studio-projekt

1. Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Välj **utveckling av Python** på arbetsbelastningar sidan och klicka sedan på **installera**. Sammanfattningsvis, kan du se att **Python 3 64-bitars (3.6.0)** väljs automatiskt för dig. Om du redan har installerat Visual Studio kan du lägga till Python arbetsbelastningen i Visual Studio-starta.
2. Efter installation och start av Visual Studio klickar du på **filen** > **ny** > **projekt**.
3. Klicka på **mallar** > **Python** > **Python programmet**, ange *myPythonProject* för namnet på projektet, välj platsen för projektet och klicka sedan på **OK**.

## <a name="install-packages"></a>Installera paket

1. I Solution Explorer under *myPythonProject*, högerklicka på **Python-miljöer**, och välj sedan **Lägg till virtuell miljö**.
2. Acceptera standardnamnet på skärmen Lägg till virtuell miljö *env*, se till att *Python 3,6 (64-bitars)* har valts för bastolk och klicka sedan på **skapa**.
3. Högerklicka på den *env* miljö som du har skapat, klicka på **Install Python Package**, ange *azure* i sökrutan och tryck på RETUR.

Du bör se i utdata windows azure-paket har installerats. 

## <a name="create-credentials"></a>Skapa autentiseringsuppgifter

Innan du startar det här steget, se till att du har en [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md). Du bör anteckna det program-ID, autentiseringsnyckeln och klient-ID som du behöver i ett senare steg.

1. Öppna *myPythonProject.py* -fil som har skapats och Lägg sedan till den här koden för att aktivera programmet att köras:

    ```python
    if __name__ == "__main__":
    ```

2. Om du vill importera den kod som krävs, lägger du till dessa instruktioner överst i filen .py:

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. Nästa i .py-fil lägger du till variabler efter importsatser så att de vanliga värden som används i koden:
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    Ersätt **prenumerations-id** med prenumerations-ID.

4. För att skapa Active Directory-autentiseringsuppgifter som du behöver göra begäranden, lägger du till den här funktionen efter variabler i .py-fil:

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    Ersätt **program-id**, **autentiseringsnyckel**, och **klient-id** med värden som du tidigare har samlat in när du skapade din Azure Active Directory-tjänstens huvudnamn.

5. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a>Skapa resurser
 
### <a name="initialize-management-clients"></a>Initiera av hanteringsklienter

Av hanteringsklienter behövs för att skapa och hantera resurser med hjälp av Python SDK i Azure. För att skapa av hanteringsklienter, Lägg till den här koden under den **om** instruktionen sedan slutet av filen .py:

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

### <a name="create-the-vm-and-supporting-resources"></a>Skapa den virtuella datorn och stöd för resurser

Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).

1. Om du vill skapa en resursgrupp, lägger du till den här funktionen efter variabler i .py-fil:

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter to continue...')
    ```

[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare att underhålla de virtuella datorerna som används av ditt program.

1. Om du vill skapa en tillgänglighetsuppsättning, lägger du till den här funktionen efter variabler i .py-fil:
   
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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter to continue...')
    ```

En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) behövs för att kommunicera med den virtuella datorn.

1. Om du vill skapa en offentlig IP-adress för den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil:

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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).

1. Om du vill skapa ett virtuellt nätverk, lägger du till den här funktionen efter variabler i .py-fil:

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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

3. Om du vill lägga till ett undernät i det virtuella nätverket, lägger du till den här funktionen efter variabler i .py-fil:
    
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
        
4. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

En virtuell dator måste ett nätverksgränssnitt för att kommunicera på det virtuella nätverket.

1. Om du vill skapa ett nätverksgränssnitt, lägger du till den här funktionen efter variabler i .py-fil:

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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

Nu när du har skapat alla stödresurser kan du skapa en virtuell dator.

1. Om du vill skapa den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil:
   
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
    > Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server. Läs mer om att välja andra bilder i [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
    > 
    > 

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter to continue...')
    ```

## <a name="perform-management-tasks"></a>Utföra administrativa uppgifter

Under livscykeln för en virtuell dator kan du vill köra hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator. Dessutom kanske du vill skapa kod för att automatisera repetitiva och komplicerade uppgifter.

### <a name="get-information-about-the-vm"></a>Hämta information om den virtuella datorn

1. För att få information om den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil:

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
2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter to continue...')
    ```

### <a name="stop-the-vm"></a>Stoppa den virtuella datorn

Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätter att debiteras för den eller stoppa en virtuell dator och frigör den. När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.

1. Om du vill stoppa den virtuella datorn utan att det har frigjorts det lägger du till den här funktionen efter variabler i .py-fil:

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    Om du vill ta bort den virtuella datorn ändra power_off anrop till den här koden:

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    stop_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="start-the-vm"></a>Starta den virtuella datorn

1. Om du vill starta den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil:

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    start_vm(compute_client)
    input('Press enter to continue...')
    ```

### <a name="resize-the-vm"></a>Ändra storlek på den virtuella datorn

Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn. Mer information finns i [VM-storlekar](sizes.md).

1. Om du vill ändra storleken på den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil:

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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter to continue...')
    ```

### <a name="add-a-data-disk-to-the-vm"></a>Lägg till en datadisk till den virtuella datorn

Virtuella datorer kan ha en eller flera [datadiskar](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som lagras som virtuella hårddiskar.

1. Om du vill lägga till en datadisk till den virtuella datorn, lägger du till den här funktionen efter variabler i .py-fil: 

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

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter to continue...')
    ```

## <a name="delete-resources"></a>Ta bort resurser

Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé att ta bort resurser som inte längre behövs. Om du vill ta bort de virtuella datorerna och alla stödresurser är allt du behöver göra resursgruppen.

1. Om du vill ta bort resursgruppen och alla resurser, lägger du till den här funktionen efter variabler i .py-fil:
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. För att anropa funktionen som du tidigare lagt till, lägger du till den här koden under den **om** uttrycket i slutet av filen .py:
   
    ```python
    delete_resources(resource_group_client)
    ```

3. Spara *myPythonProject.py*.

## <a name="run-the-application"></a>Köra programmet

1. Klicka på att köra konsolprogrammet **starta** i Visual Studio.

2. Tryck på **RETUR** när status för varje resurs som har returnerats. I statusinformation, bör du se en **lyckades** Etableringsstatus. När den virtuella datorn skapas, har du möjlighet att ta bort alla resurser som du skapar. Innan du trycker på **ange** om du vill börja ta bort resurser, du kan ta några minuter för att verifiera skapas i Azure-portalen. Du kan behöva uppdatera bladet om du vill se nya resurser i Azure-portalen öppnas.  

    Det bör ta ungefär fem minuter för den här konsolen programmet helt från början till slut. Det kan ta flera minuter när programmet har slutförts innan alla resurser och resursgruppen tas bort.


## <a name="next-steps"></a>Nästa steg

- Om det var problem med distributionen, är nästa steg att titta på [Felsöka resursgruppdistribueringar med Azure-portalen](../../resource-manager-troubleshoot-deployments-portal.md)
- Lär dig mer om den [Azure Python-bibliotek](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)

