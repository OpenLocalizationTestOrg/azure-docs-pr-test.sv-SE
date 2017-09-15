# <a name="using-managed-disks-in-azure-resource-manager-templates"></a><span data-ttu-id="ae471-101">Med hjälp av hanterade diskar i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="ae471-101">Using Managed Disks in Azure Resource Manager Templates</span></span>

<span data-ttu-id="ae471-102">Det här dokumentet går igenom skillnader mellan hanterade och ohanterade diskar när du använder Azure Resource Manager-mallar för att etablera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ae471-102">This document walks through the differences between managed and unmanaged disks when using Azure Resource Manager templates to provision virtual machines.</span></span> <span data-ttu-id="ae471-103">Detta hjälper dig att uppdatera befintliga mallar som använder ohanterade diskar till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="ae471-103">This will help you to update existing templates that are using unmanaged Disks to managed disks.</span></span> <span data-ttu-id="ae471-104">För referens anger vi använder den [101-vm-enkel-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) mallen som en vägledning.</span><span class="sxs-lookup"><span data-stu-id="ae471-104">For reference, we are using the [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) template as a guide.</span></span> <span data-ttu-id="ae471-105">Du kan se mallen använder både [hanterade diskar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) och en tidigare version med hjälp av [ohanterad diskar](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) om du vill jämföra dem direkt.</span><span class="sxs-lookup"><span data-stu-id="ae471-105">You can see the template using both [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) and a prior version using [unmanaged disks](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) if you'd like to directly compare them.</span></span>

## <a name="unmanaged-disks-template-formatting"></a><span data-ttu-id="ae471-106">Ohanterad diskar mallen formatering</span><span class="sxs-lookup"><span data-stu-id="ae471-106">Unmanaged Disks template formatting</span></span>

<span data-ttu-id="ae471-107">Om du vill börja, vi ta en titt på hur ohanterade diskar har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="ae471-107">To begin, we take a look at how unmanaged disks are deployed.</span></span> <span data-ttu-id="ae471-108">När du skapar en ohanterad diskar, behöver ett lagringskonto för VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="ae471-108">When creating unmanaged disks, you need a storage account to hold the VHD files.</span></span> <span data-ttu-id="ae471-109">Du kan skapa ett nytt lagringskonto eller använda en som redan finns.</span><span class="sxs-lookup"><span data-stu-id="ae471-109">You can create a new storage account or use one that already exists.</span></span> <span data-ttu-id="ae471-110">Den här artikeln visar hur du skapar ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="ae471-110">This article will show you how to create a new storage account.</span></span> <span data-ttu-id="ae471-111">För att åstadkomma detta behöver du en lagringsresurs konto i blocket resurser som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="ae471-111">To accomplish this, you need a storage account resource in the resources block as shown below.</span></span>

```
{
    "type": "Microsoft.Storage/storageAccounts",
    "name": "[variables('storageAccountName')]",
    "apiVersion": "2016-01-01",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "kind": "Storage",
    "properties": {}
}
```

<span data-ttu-id="ae471-112">I det virtuella datorobjektet måste vi ett beroende på lagringskontot för att se till att den har skapats innan den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ae471-112">Within the virtual machine object, we need a dependency on the storage account to ensure that it's created before the virtual machine.</span></span> <span data-ttu-id="ae471-113">I den `storageProfile` avsnittet vi sedan ange fullständiga URI för VHD-platsen som refererar till lagringskontot och behövs för OS-disken och eventuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="ae471-113">Within the `storageProfile` section, we then specify the full URI of the VHD location, which references the storage account and is needed for the OS disk and any data disks.</span></span> 

```
{
    "apiVersion": "2015-06-15",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
    "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
    "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "name": "osdisk",
                "vhd": {
                    "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/osdisk.vhd')]"
                },
                "caching": "ReadWrite",
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "name": "datadisk1",
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "vhd": {
                        "uri": "[concat(reference(resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))).primaryEndpoints.blob, 'vhds/datadisk1.vhd')]"
                    },
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

## <a name="managed-disks-template-formatting"></a><span data-ttu-id="ae471-114">Hanterade diskar mallen formatering</span><span class="sxs-lookup"><span data-stu-id="ae471-114">Managed disks template formatting</span></span>

<span data-ttu-id="ae471-115">Med Azure hanterade diskar disken blir en översta resurs och kräver inte längre ett lagringskonto skapas av användaren.</span><span class="sxs-lookup"><span data-stu-id="ae471-115">With Azure Managed Disks, the disk becomes a top-level resource and no longer requires a storage account to be created by the user.</span></span> <span data-ttu-id="ae471-116">Hanterade diskar har först exponeras i den `2016-04-30-preview` API-versionen som de är tillgängliga i alla efterföljande API-versioner och nu är standard disktyp.</span><span class="sxs-lookup"><span data-stu-id="ae471-116">Managed disks were first exposed in the `2016-04-30-preview` API version, they are available in all subsequent API versions and are now the default disk type.</span></span> <span data-ttu-id="ae471-117">I följande avsnitt igenom standardinställningarna och innehåller information om hur du anpassar ytterligare diskar.</span><span class="sxs-lookup"><span data-stu-id="ae471-117">The following sections walk through the default settings and detail how to further customize your disks.</span></span>

> [!NOTE]
> <span data-ttu-id="ae471-118">Det rekommenderas att använda en API-version senare än `2016-04-30-preview` eftersom viktiga förändringar mellan `2016-04-30-preview` och `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="ae471-118">It is recommended to use an API version later than `2016-04-30-preview` as there were breaking changes between `2016-04-30-preview` and `2017-03-30`.</span></span>
>
>

### <a name="default-managed-disk-settings"></a><span data-ttu-id="ae471-119">Standardinställningarna för hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="ae471-119">Default managed disk settings</span></span>

<span data-ttu-id="ae471-120">Om du vill skapa en virtuell dator med hanterade diskar, behöver du inte längre skapa lagring kontot resurs och uppdatera din virtuella datorresursen enligt följande.</span><span class="sxs-lookup"><span data-stu-id="ae471-120">To create a VM with managed disks, you no longer need to create the storage account resource and can update your virtual machine resource as follows.</span></span> <span data-ttu-id="ae471-121">Observera att särskilt den `apiVersion` återspeglar `2017-03-30` och `osDisk` och `dataDisks` inte längre referera till en specifik URI för den virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ae471-121">Specifically note that the `apiVersion` reflects `2017-03-30` and the `osDisk` and `dataDisks` no longer refer to a specific URI for the VHD.</span></span> <span data-ttu-id="ae471-122">När du distribuerar utan att ange ytterligare egenskaper som använder disken [Standard LRS lagring](../articles/storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="ae471-122">When deploying without specifying additional properties, the disk will use [Standard LRS storage](../articles/storage/common/storage-redundancy.md).</span></span> <span data-ttu-id="ae471-123">Om inget namn anges tar format `<VMName>_OsDisk_1_<randomstring>` för OS-disken och `<VMName>_disk<#>_<randomstring>` för varje datadisk.</span><span class="sxs-lookup"><span data-stu-id="ae471-123">If no name is specified, it takes the format of `<VMName>_OsDisk_1_<randomstring>` for the OS disk and `<VMName>_disk<#>_<randomstring>` for each data disk.</span></span> <span data-ttu-id="ae471-124">Azure disk encryption är inaktiverat som standard. cachelagring är läsning och skrivning för OS-disken och ingen för datadiskar.</span><span class="sxs-lookup"><span data-stu-id="ae471-124">By default, Azure disk encryption is disabled; caching is Read/Write for the OS disk and None for data disks.</span></span> <span data-ttu-id="ae471-125">Du kan se i exemplet nedan har fortfarande ett beroende som storage-konto, även om detta är endast avsedd för lagring av diagnostik och behövs inte för disklagring.</span><span class="sxs-lookup"><span data-stu-id="ae471-125">You may notice in the example below there is still a storage account dependency, though this is only for storage of diagnostics and is not needed for disk storage.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "diskSizeGB": 1023,
                    "lun": 0,
                    "createOption": "Empty"
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="using-a-top-level-managed-disk-resource"></a><span data-ttu-id="ae471-126">Med hjälp av en översta hanterade diskresurs</span><span class="sxs-lookup"><span data-stu-id="ae471-126">Using a top-level managed disk resource</span></span>

<span data-ttu-id="ae471-127">Du kan skapa en översta diskresurs och bifoga den som en del av den virtuella dator skapandet som ett alternativ till att ange diskkonfigurationen i det virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="ae471-127">As an alternative to specifying the disk configuration in the virtual machine object, you can create a top-level disk resource and attach it as part of the virtual machine creation.</span></span> <span data-ttu-id="ae471-128">Vi kan till exempel skapa en diskresurs enligt följande för att använda som en datadisk.</span><span class="sxs-lookup"><span data-stu-id="ae471-128">For example, we can create a disk resource as follows to use as a data disk.</span></span>

```
{
    "type": "Microsoft.Compute/disks",
    "name": "[concat(variables('vmName'),'-datadisk1')]",
    "apiVersion": "2017-03-30",
    "location": "[resourceGroup().location]",
    "sku": {
        "name": "Standard_LRS"
    },
    "properties": {
        "creationData": {
            "createOption": "Empty"
        },
        "diskSizeGB": 1023
    }
}
```

<span data-ttu-id="ae471-129">I det Virtuella datorobjektet, kan vi sedan referera diskobjektet ska anslutas.</span><span class="sxs-lookup"><span data-stu-id="ae471-129">Within the VM object, we can then reference this disk object to be attached.</span></span> <span data-ttu-id="ae471-130">Ange resurs-ID för den hantera disken som vi skapade i den `managedDisk` egenskapen kan den bifogade filen på disken som den virtuella datorn skapas.</span><span class="sxs-lookup"><span data-stu-id="ae471-130">Specifying the resource ID of the managed disk we created in the `managedDisk` property allows the attachment of the disk as the VM is created.</span></span> <span data-ttu-id="ae471-131">Observera att den `apiVersion` för den virtuella datorn resursen har angetts till `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="ae471-131">Note that the `apiVersion` for the VM resource is set to `2017-03-30`.</span></span> <span data-ttu-id="ae471-132">Tänk också på att vi har skapat ett beroende för diskresursen för att säkerställa att den har skapats innan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ae471-132">Also note that we've created a dependency on the disk resource to ensure it's successfully created before VM creation.</span></span> 

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/virtualMachines",
    "name": "[variables('vmName')]",
    "location": "[resourceGroup().location]",
    "dependsOn": [
        "[resourceId('Microsoft.Storage/storageAccounts/', variables('storageAccountName'))]",
        "[resourceId('Microsoft.Network/networkInterfaces/', variables('nicName'))]",
        "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
    ],
    "properties": {
        "hardwareProfile": {...},
        "osProfile": {...},
        "storageProfile": {
            "imageReference": {
                "publisher": "MicrosoftWindowsServer",
                "offer": "WindowsServer",
                "sku": "[parameters('windowsOSVersion')]",
                "version": "latest"
            },
            "osDisk": {
                "createOption": "FromImage"
            },
            "dataDisks": [
                {
                    "lun": 0,
                    "name": "[concat(variables('vmName'),'-datadisk1')]",
                    "createOption": "attach",
                    "managedDisk": {
                        "id": "[resourceId('Microsoft.Compute/disks/', concat(variables('vmName'),'-datadisk1'))]"
                    }
                }
            ]
        },
        "networkProfile": {...},
        "diagnosticsProfile": {...}
    }
}
```

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a><span data-ttu-id="ae471-133">Skapa hanterade tillgänglighetsuppsättningar med virtuella datorer med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="ae471-133">Create managed availability sets with VMs using managed disks</span></span>

<span data-ttu-id="ae471-134">För att skapa hanterade tillgänglighetsuppsättningar med virtuella datorer med hjälp av hanterade diskar, lägga till den `sku` objekt tillgängligheten resursen och den `name` egenskapen `Aligned`.</span><span class="sxs-lookup"><span data-stu-id="ae471-134">To create managed availability sets with VMs using managed disks, add the `sku` object to the availability set resource and set the `name` property to `Aligned`.</span></span> <span data-ttu-id="ae471-135">Detta säkerställer att alla diskar för varje virtuell dator är tillräckligt isolerade från varandra för att undvika enskilda felpunkter.</span><span class="sxs-lookup"><span data-stu-id="ae471-135">This ensures that the disks for each VM are sufficiently isolated from each other to avoid single points of failure.</span></span> <span data-ttu-id="ae471-136">Observera att den `apiVersion` för resursen tillgänglighetsuppsättning är inställd på `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="ae471-136">Also note that the `apiVersion` for the availability set resource is set to `2017-03-30`.</span></span>

```
{
    "apiVersion": "2017-03-30",
    "type": "Microsoft.Compute/availabilitySets",
    "location": "[resourceGroup().location]",
    "name": "[variables('avSetName')]",
    "properties": {
        "PlatformUpdateDomainCount": 3,
        "PlatformFaultDomainCount": 2
    },
    "sku": {
        "name": "Aligned"
    }
}
```

### <a name="additional-scenarios-and-customizations"></a><span data-ttu-id="ae471-137">Fler scenarier och anpassningar</span><span class="sxs-lookup"><span data-stu-id="ae471-137">Additional scenarios and customizations</span></span>

<span data-ttu-id="ae471-138">Om du vill hitta fullständig information om REST API-specifikationerna, granska den [skapar du en hanterad disk REST API-dokumentation](/rest/api/manageddisks/disks/disks-create-or-update).</span><span class="sxs-lookup"><span data-stu-id="ae471-138">To find full information on the REST API specifications, please review the [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update).</span></span> <span data-ttu-id="ae471-139">Du hittar fler scenarier, såväl som standard och godkända värden som ska skickas till API: N via mall-distributioner.</span><span class="sxs-lookup"><span data-stu-id="ae471-139">You will find additional scenarios, as well as default and acceptable values that can be submitted to the API through template deployments.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="ae471-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ae471-140">Next steps</span></span>

* <span data-ttu-id="ae471-141">Fullständig mallar som använder hanterade diskar finns på följande länkar för Azure Quickstart lagringsplatsen.</span><span class="sxs-lookup"><span data-stu-id="ae471-141">For full templates that use managed disks visit the following Azure Quickstart Repo links.</span></span>
    * [<span data-ttu-id="ae471-142">Windows virtuell dator med hanterad disk</span><span class="sxs-lookup"><span data-stu-id="ae471-142">Windows VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [<span data-ttu-id="ae471-143">Linux VM med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="ae471-143">Linux VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [<span data-ttu-id="ae471-144">Fullständig lista över mallar för hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="ae471-144">Full list of managed disk templates</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="ae471-145">Besök den [hanterade diskar översikt över Azure](../articles/virtual-machines/windows/managed-disks-overview.md) dokumentet för mer information om hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="ae471-145">Visit the [Azure Managed Disks Overview](../articles/virtual-machines/windows/managed-disks-overview.md) document to learn more about managed disks.</span></span>
* <span data-ttu-id="ae471-146">Granska referensdokumentationen mall för virtuella datorresurser genom att besöka den [Microsoft.Compute/virtualMachines mallreferensen](/templates/microsoft.compute/virtualmachines) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ae471-146">Review the template reference documentation for virtual machine resources by visiting the [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines) document.</span></span>
* <span data-ttu-id="ae471-147">Granska referensdokumentationen mall för diskresurser genom att besöka den [Microsoft.Compute/disks mallreferensen](/templates/microsoft.compute/disks) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="ae471-147">Review the template reference documentation for disk resources by visiting the [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks) document.</span></span>
 
