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
# <a name="create-and-manage-windows-vms-in-azure-using-python"></a><span data-ttu-id="e5d2d-103">Skapa och hantera virtuella Windows-datorer i Azure med Python</span><span class="sxs-lookup"><span data-stu-id="e5d2d-103">Create and manage Windows VMs in Azure using Python</span></span>

<span data-ttu-id="e5d2d-104">En [Azure virtuella](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) måste flera stödjande Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-104">An [Azure Virtual Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) needs several supporting Azure resources.</span></span> <span data-ttu-id="e5d2d-105">Den här artikeln beskriver hur du skapar, hantera och ta bort VM-resurser med hjälp av Python.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-105">This article covers creating, managing, and deleting VM resources using Python.</span></span> <span data-ttu-id="e5d2d-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e5d2d-107">Skapa ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e5d2d-107">Create a Visual Studio project</span></span>
> * <span data-ttu-id="e5d2d-108">Installera paket</span><span class="sxs-lookup"><span data-stu-id="e5d2d-108">Install packages</span></span>
> * <span data-ttu-id="e5d2d-109">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="e5d2d-109">Create credentials</span></span>
> * <span data-ttu-id="e5d2d-110">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="e5d2d-110">Create resources</span></span>
> * <span data-ttu-id="e5d2d-111">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="e5d2d-111">Perform management tasks</span></span>
> * <span data-ttu-id="e5d2d-112">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="e5d2d-112">Delete resources</span></span>
> * <span data-ttu-id="e5d2d-113">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="e5d2d-113">Run hello application</span></span>

<span data-ttu-id="e5d2d-114">Det tar cirka 20 minuter toodo dessa steg.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-114">It takes about 20 minutes toodo these steps.</span></span>

## <a name="create-a-visual-studio-project"></a><span data-ttu-id="e5d2d-115">Skapa ett Visual Studio-projekt</span><span class="sxs-lookup"><span data-stu-id="e5d2d-115">Create a Visual Studio project</span></span>

1. <span data-ttu-id="e5d2d-116">Om du inte redan gjort installera [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-116">If you haven't already, install [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio).</span></span> <span data-ttu-id="e5d2d-117">Välj **utveckling av Python** på hello arbetsbelastningar sidan och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-117">Select **Python development** on hello Workloads page, and then click **Install**.</span></span> <span data-ttu-id="e5d2d-118">I hello sammanfattning, kan du se att **Python 3 64-bitars (3.6.0)** väljs automatiskt för dig.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-118">In hello summary, you can see that **Python 3 64-bit (3.6.0)** is automatically selected for you.</span></span> <span data-ttu-id="e5d2d-119">Om du redan har installerat Visual Studio kan du lägga till hello Python arbetsbelastning med hello Visual Studio starta.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-119">If you have already installed Visual Studio, you can add hello Python workload using hello Visual Studio Launcher.</span></span>
2. <span data-ttu-id="e5d2d-120">Efter installation och start av Visual Studio klickar du på **filen** > **ny** > **projekt**.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-120">After installing and starting Visual Studio, click **File** > **New** > **Project**.</span></span>
3. <span data-ttu-id="e5d2d-121">Klicka på **mallar** > **Python** > **Python programmet**, ange *myPythonProject* för hello namn hello Project välja hello platsen för hello projektet och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-121">Click **Templates** > **Python** > **Python Application**, enter *myPythonProject* for hello name of hello project, select hello location of hello project, and then click **OK**.</span></span>

## <a name="install-packages"></a><span data-ttu-id="e5d2d-122">Installera paket</span><span class="sxs-lookup"><span data-stu-id="e5d2d-122">Install packages</span></span>

1. <span data-ttu-id="e5d2d-123">I Solution Explorer under *myPythonProject*, högerklicka på **Python-miljöer**, och välj sedan **Lägg till virtuell miljö**.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-123">In Solution Explorer, under *myPythonProject*, right-click **Python Environments**, and then select **Add virtual environment**.</span></span>
2. <span data-ttu-id="e5d2d-124">Acceptera hello standardnamnet på hello-skärmen Lägg till virtuell miljö *env*, se till att *Python 3,6 (64-bitars)* har valts för hello bastolk och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-124">On hello Add Virtual Environment screen, accept hello default name of *env*, make sure that *Python 3.6 (64-bit)* is selected for hello base interpreter, and then click **Create**.</span></span>
3. <span data-ttu-id="e5d2d-125">Högerklicka på hello *env* miljö som du har skapat, klicka på **Install Python Package**, ange *azure* i hello sökrutan och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-125">Right-click hello *env* environment that you created, click **Install Python Package**, enter *azure* in hello search box, and then press Enter.</span></span>

<span data-ttu-id="e5d2d-126">Du bör se i hello utdata windows hello azure-paket har installerats.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-126">You should see in hello output windows that hello azure packages were successfully installed.</span></span> 

## <a name="create-credentials"></a><span data-ttu-id="e5d2d-127">Skapa autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="e5d2d-127">Create credentials</span></span>

<span data-ttu-id="e5d2d-128">Innan du startar det här steget, se till att du har en [Active Directory-tjänstens huvudnamn](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-128">Before you start this step, make sure that you have an [Active Directory service principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md).</span></span> <span data-ttu-id="e5d2d-129">Du bör anteckna hello program-ID och autentiseringsnyckel hello hello klient-ID som du behöver i ett senare steg.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-129">You should also record hello application ID, hello authentication key, and hello tenant ID that you need in a later step.</span></span>

1. <span data-ttu-id="e5d2d-130">Öppna *myPythonProject.py* fil som har skapats och Lägg sedan till den här koden tooenable toorun ditt program:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-130">Open *myPythonProject.py* file that was created, and then add this code tooenable your application toorun:</span></span>

    ```python
    if __name__ == "__main__":
    ```

2. <span data-ttu-id="e5d2d-131">tooimport hello kod som krävs, lägger du till dessa instruktioner toohello överkant hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-131">tooimport hello code that is needed, add these statements toohello top of hello .py file:</span></span>

    ```python
    from azure.common.credentials import ServicePrincipalCredentials
    from azure.mgmt.resource import ResourceManagementClient
    from azure.mgmt.compute import ComputeManagementClient
    from azure.mgmt.network import NetworkManagementClient
    from azure.mgmt.compute.models import DiskCreateOption
    ```

3. <span data-ttu-id="e5d2d-132">Nästa hello .py-fil, lägga till variabler när hello importuttryck toospecify vanliga värden används i hello kod:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-132">Next in hello .py file, add variables after hello import statements toospecify common values used in hello code:</span></span>
   
    ```
    SUBSCRIPTION_ID = 'subscription-id'
    GROUP_NAME = 'myResourceGroup'
    LOCATION = 'westus'
    VM_NAME = 'myVM'
    ```

    <span data-ttu-id="e5d2d-133">Ersätt **prenumerations-id** med prenumerations-ID.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-133">Replace **subscription-id** with your subscription identifier.</span></span>

4. <span data-ttu-id="e5d2d-134">toocreate hello Active Directory-autentiseringsuppgifter som du behöver toomake begäranden, lägger till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-134">toocreate hello Active Directory credentials that you need toomake requests, add this function after hello variables in hello .py file:</span></span>

    ```python
    def get_credentials():
        credentials = ServicePrincipalCredentials(
            client_id = 'application-id',
            secret = 'authentication-key',
            tenant = 'tenant-id'
        )

        return credentials
    ```

    <span data-ttu-id="e5d2d-135">Ersätt **program-id**, **autentiseringsnyckel**, och **klient-id** med hello-värden som du tidigare har samlat in när du skapade din Azure Active Directory tjänstens huvudnamn.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-135">Replace **application-id**, **authentication-key**, and **tenant-id** with hello values that you previously collected when you created your Azure Active Directory service principal.</span></span>

5. <span data-ttu-id="e5d2d-136">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-136">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    credentials = get_credentials()
    ```

## <a name="create-resources"></a><span data-ttu-id="e5d2d-137">Skapa resurser</span><span class="sxs-lookup"><span data-stu-id="e5d2d-137">Create resources</span></span>
 
### <a name="initialize-management-clients"></a><span data-ttu-id="e5d2d-138">Initiera av hanteringsklienter</span><span class="sxs-lookup"><span data-stu-id="e5d2d-138">Initialize management clients</span></span>

<span data-ttu-id="e5d2d-139">Av hanteringsklienter är nödvändiga toocreate och hantera resurser med hjälp av hello Python SDK i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-139">Management clients are needed toocreate and manage resources using hello Python SDK in Azure.</span></span> <span data-ttu-id="e5d2d-140">toocreate hello hantering av klienter, Lägg till denna kod under hello **om** instruktionen sedan slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-140">toocreate hello management clients, add this code under hello **if** statement at then end of hello .py file:</span></span>

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

### <a name="create-hello-vm-and-supporting-resources"></a><span data-ttu-id="e5d2d-141">Skapa hello VM och stöd för resurser</span><span class="sxs-lookup"><span data-stu-id="e5d2d-141">Create hello VM and supporting resources</span></span>

<span data-ttu-id="e5d2d-142">Alla resurser måste finnas i en [resursgruppen](../../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-142">All resources must be contained in a [Resource group](../../azure-resource-manager/resource-group-overview.md).</span></span>

1. <span data-ttu-id="e5d2d-143">toocreate en resursgrupp, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-143">toocreate a resource group, add this function after hello variables in hello .py file:</span></span>

    ```python
    def create_resource_group(resource_group_client):
        resource_group_params = { 'location':LOCATION }
        resource_group_result = resource_group_client.resource_groups.create_or_update(
            GROUP_NAME, 
            resource_group_params
        )
    ```

2. <span data-ttu-id="e5d2d-144">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-144">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_resource_group(resource_group_client)
    input('Resource group created. Press enter toocontinue...')
    ```

<span data-ttu-id="e5d2d-145">[Tillgänglighetsuppsättningar](tutorial-availability-sets.md) gör det enklare för dig toomaintain hello virtuella datorer som används av ditt program.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-145">[Availability sets](tutorial-availability-sets.md) make it easier for you toomaintain hello virtual machines used by your application.</span></span>

1. <span data-ttu-id="e5d2d-146">toocreate tillgänglighet ange, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-146">toocreate an availability set, add this function after hello variables in hello .py file:</span></span>
   
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

2. <span data-ttu-id="e5d2d-147">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-147">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    create_availability_set(compute_client)
    print("------------------------------------------------------")
    input('Availability set created. Press enter toocontinue...')
    ```

<span data-ttu-id="e5d2d-148">En [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) är nödvändiga toocommunicate med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-148">A [Public IP address](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is needed toocommunicate with hello virtual machine.</span></span>

1. <span data-ttu-id="e5d2d-149">toocreate offentlig IP-adress för hello virtuell dator, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-149">toocreate a public IP address for hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="e5d2d-150">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-150">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_public_ip_address(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="e5d2d-151">En virtuell dator måste vara i ett undernät för en [för virtuella nätverk](../../virtual-network/virtual-networks-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-151">A virtual machine must be in a subnet of a [Virtual network](../../virtual-network/virtual-networks-overview.md).</span></span>

1. <span data-ttu-id="e5d2d-152">toocreate ett virtuellt nätverk, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-152">toocreate a virtual network, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="e5d2d-153">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-153">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_vnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

3. <span data-ttu-id="e5d2d-154">tooadd en toohello virtuella undernätverk, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-154">tooadd a subnet toohello virtual network, add this function after hello variables in hello .py file:</span></span>
    
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
        
4. <span data-ttu-id="e5d2d-155">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-155">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    creation_result = create_subnet(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="e5d2d-156">En virtuell dator måste en network interface toocommunicate hello virtuella nätverket.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-156">A virtual machine needs a network interface toocommunicate on hello virtual network.</span></span>

1. <span data-ttu-id="e5d2d-157">toocreate nätverksgränssnitt, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-157">toocreate a network interface, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="e5d2d-158">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-158">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_nic(network_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

<span data-ttu-id="e5d2d-159">Nu när du har skapat alla hello stöder resurser kan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-159">Now that you created all hello supporting resources, you can create a virtual machine.</span></span>

1. <span data-ttu-id="e5d2d-160">toocreate Hej virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-160">toocreate hello virtual machine, add this function after hello variables in hello .py file:</span></span>
   
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
    > <span data-ttu-id="e5d2d-161">Den här guiden skapar en virtuell dator som kör en version av operativsystemet Windows Server för hello.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-161">This tutorial creates a virtual machine running a version of hello Windows Server operating system.</span></span> <span data-ttu-id="e5d2d-162">toolearn mer information om hur du väljer andra bilder, se [analysera och välja avbildningar för virtuell Azure-dator med Windows PowerShell och hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-162">toolearn more about selecting other images, see [Navigate and select Azure virtual machine images with Windows PowerShell and hello Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
    > 
    > 

2. <span data-ttu-id="e5d2d-163">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-163">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    creation_result = create_vm(network_client, compute_client)
    print("------------------------------------------------------")
    print(creation_result)
    input('Press enter toocontinue...')
    ```

## <a name="perform-management-tasks"></a><span data-ttu-id="e5d2d-164">Utföra administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="e5d2d-164">Perform management tasks</span></span>

<span data-ttu-id="e5d2d-165">Du kanske vill toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator under hello livscykeln för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-165">During hello lifecycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="e5d2d-166">Dessutom kan du toocreate kod tooautomate repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-166">Additionally, you may want toocreate code tooautomate repetitive or complex tasks.</span></span>

### <a name="get-information-about-hello-vm"></a><span data-ttu-id="e5d2d-167">Hämta information om hello VM</span><span class="sxs-lookup"><span data-stu-id="e5d2d-167">Get information about hello VM</span></span>

1. <span data-ttu-id="e5d2d-168">tooget information om hello virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-168">tooget information about hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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
2. <span data-ttu-id="e5d2d-169">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-169">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    get_vm(compute_client)
    print("------------------------------------------------------")
    input('Press enter toocontinue...')
    ```

### <a name="stop-hello-vm"></a><span data-ttu-id="e5d2d-170">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="e5d2d-170">Stop hello VM</span></span>

<span data-ttu-id="e5d2d-171">Du kan stoppa en virtuell dator och behålla alla inställningar, men fortsätta toobe debiteras för den eller stoppa en virtuell dator och frigör den.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-171">You can stop a virtual machine and keep all its settings, but continue toobe charged for it, or you can stop a virtual machine and deallocate it.</span></span> <span data-ttu-id="e5d2d-172">När en virtuell dator har frigjorts alla resurser som är associerade med den är också frigjord och fakturering avslutas för den.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-172">When a virtual machine is deallocated, all resources associated with it are also deallocated and billing ends for it.</span></span>

1. <span data-ttu-id="e5d2d-173">toostop hello virtuell dator utan att det har frigjorts, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-173">toostop hello virtual machine without deallocating it, add this function after hello variables in hello .py file:</span></span>

    ```python
    def stop_vm(compute_client):
        compute_client.virtual_machines.power_off(GROUP_NAME, VM_NAME)
    ```

    <span data-ttu-id="e5d2d-174">Om du vill toodeallocate hello virtuella datorn, ändra hello power_off anropet toothis koden:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-174">If you want toodeallocate hello virtual machine, change hello power_off call toothis code:</span></span>

    ```python
    compute_client.virtual_machines.deallocate(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="e5d2d-175">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-175">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    stop_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="start-hello-vm"></a><span data-ttu-id="e5d2d-176">Starta hello VM</span><span class="sxs-lookup"><span data-stu-id="e5d2d-176">Start hello VM</span></span>

1. <span data-ttu-id="e5d2d-177">toostart Hej virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-177">toostart hello virtual machine, add this function after hello variables in hello .py file:</span></span>

    ```python
    def start_vm(compute_client):
        compute_client.virtual_machines.start(GROUP_NAME, VM_NAME)
    ```

2. <span data-ttu-id="e5d2d-178">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-178">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    start_vm(compute_client)
    input('Press enter toocontinue...')
    ```

### <a name="resize-hello-vm"></a><span data-ttu-id="e5d2d-179">Ändra storlek på hello VM</span><span class="sxs-lookup"><span data-stu-id="e5d2d-179">Resize hello VM</span></span>

<span data-ttu-id="e5d2d-180">Många aspekter av distributionen bör övervägas när du funderar över en storlek för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-180">Many aspects of deployment should be considered when deciding on a size for your virtual machine.</span></span> <span data-ttu-id="e5d2d-181">Mer information finns i [VM-storlekar](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="e5d2d-181">For more information, see [VM sizes](sizes.md).</span></span>

1. <span data-ttu-id="e5d2d-182">toochange hello storleken på hello virtuella datorn, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-182">toochange hello size of hello virtual machine, add this function after hello variables in hello .py file:</span></span>

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

2. <span data-ttu-id="e5d2d-183">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-183">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    update_result = update_vm(compute_client)
    print("------------------------------------------------------")
    print(update_result)
    input('Press enter toocontinue...')
    ```

### <a name="add-a-data-disk-toohello-vm"></a><span data-ttu-id="e5d2d-184">Lägg till en data disk toohello VM</span><span class="sxs-lookup"><span data-stu-id="e5d2d-184">Add a data disk toohello VM</span></span>

<span data-ttu-id="e5d2d-185">Virtuella datorer kan ha en eller flera [datadiskar](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) som lagras som virtuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-185">Virtual machines can have one or more [data disks](about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) that are stored as VHDs.</span></span>

1. <span data-ttu-id="e5d2d-186">tooadd en data disk toohello virtuell dator, Lägg till den här funktionen efter hello variabler i hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-186">tooadd a data disk toohello virtual machine, add this function after hello variables in hello .py file:</span></span> 

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

2. <span data-ttu-id="e5d2d-187">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-187">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>

    ```python
    add_result = add_datadisk(compute_client)
    print("------------------------------------------------------")
    print(add_result)
    input('Press enter toocontinue...')
    ```

## <a name="delete-resources"></a><span data-ttu-id="e5d2d-188">Ta bort resurser</span><span class="sxs-lookup"><span data-stu-id="e5d2d-188">Delete resources</span></span>

<span data-ttu-id="e5d2d-189">Eftersom du debiteras för de resurser som används i Azure, men det är alltid en bra idé toodelete resurser som inte längre behövs.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-189">Because you are charged for resources used in Azure, it's always a good practice toodelete resources that are no longer needed.</span></span> <span data-ttu-id="e5d2d-190">Om du vill toodelete hello virtuella datorer och alla hello stöder resurser, alla har toodo är delete hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-190">If you want toodelete hello virtual machines and all hello supporting resources, all you have toodo is delete hello resource group.</span></span>

1. <span data-ttu-id="e5d2d-191">Lägg till den här funktionen efter hello variabler i hello .py-fil toodelete hello resursgruppen och alla resurser:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-191">toodelete hello resource group and all resources, add this function after hello variables in hello .py file:</span></span>
   
    ```python
    def delete_resources(resource_group_client):
        resource_group_client.resource_groups.delete(GROUP_NAME)
    ```

2. <span data-ttu-id="e5d2d-192">toocall hello funktion som du tidigare lagt till, Lägg till denna kod under hello **om** instruktionen hello slutet av hello .py-fil:</span><span class="sxs-lookup"><span data-stu-id="e5d2d-192">toocall hello function that you previously added, add this code under hello **if** statement at hello end of hello .py file:</span></span>
   
    ```python
    delete_resources(resource_group_client)
    ```

3. <span data-ttu-id="e5d2d-193">Spara *myPythonProject.py*.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-193">Save *myPythonProject.py*.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="e5d2d-194">Kör programmet hello</span><span class="sxs-lookup"><span data-stu-id="e5d2d-194">Run hello application</span></span>

1. <span data-ttu-id="e5d2d-195">toorun hello-konsolprogram klickar du på **starta** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-195">toorun hello console application, click **Start** in Visual Studio.</span></span>

2. <span data-ttu-id="e5d2d-196">Tryck på **RETUR** när returneras hello status för varje resurs.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-196">Press **Enter** after hello status of each resource is returned.</span></span> <span data-ttu-id="e5d2d-197">I hello statusinformation, bör du se en **lyckades** Etableringsstatus.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-197">In hello status information, you should see a **Succeeded** provisioning state.</span></span> <span data-ttu-id="e5d2d-198">När hello virtuell dator skapas har du hello möjlighet toodelete alla hello-resurser som du skapar.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-198">After hello virtual machine is created, you have hello opportunity toodelete all hello resources that you create.</span></span> <span data-ttu-id="e5d2d-199">Innan du trycker på **RETUR** toostart ta bort resurser, du kan ta några minuter tooverify skapas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-199">Before you press **Enter** toostart deleting resources, you could take a few minutes tooverify their creation in hello Azure portal.</span></span> <span data-ttu-id="e5d2d-200">Om du har hello Azure portal öppna kanske toorefresh hello bladet toosee nya resurser.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-200">If you have hello Azure portal open, you might have toorefresh hello blade toosee new resources.</span></span>  

    <span data-ttu-id="e5d2d-201">Det bör ta ungefär fem minuter för den här konsolen programmet toorun helt från start toofinish.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-201">It should take about five minutes for this console application toorun completely from start toofinish.</span></span> <span data-ttu-id="e5d2d-202">Det kan ta flera minuter efter hello program har slutförts innan alla hello-resurser och hello resursgruppen tas bort.</span><span class="sxs-lookup"><span data-stu-id="e5d2d-202">It may take several minutes after hello application has finished before all hello resources and hello resource group are deleted.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e5d2d-203">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5d2d-203">Next steps</span></span>

- <span data-ttu-id="e5d2d-204">Om det fanns problem med hello distribution, nästa steg är toolook på [felsöka distributionen av resursgrupper med Azure-portalen](../../resource-manager-troubleshoot-deployments-portal.md)</span><span class="sxs-lookup"><span data-stu-id="e5d2d-204">If there were issues with hello deployment, a next step would be toolook at [Troubleshooting resource group deployments with Azure portal](../../resource-manager-troubleshoot-deployments-portal.md)</span></span>
- <span data-ttu-id="e5d2d-205">Mer information om hello [Azure Python-bibliotek](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span><span class="sxs-lookup"><span data-stu-id="e5d2d-205">Learn more about hello [Azure Python Library](https://docs.microsoft.com/python/api/overview/azure/?view=azure-python)</span></span>

