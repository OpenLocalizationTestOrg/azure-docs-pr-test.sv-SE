# <a name="using-managed-disks-in-azure-resource-manager-templates"></a><span data-ttu-id="d3c4f-101">Med hjälp av hanterade diskar i Azure Resource Manager-mallar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-101">Using Managed Disks in Azure Resource Manager Templates</span></span>

<span data-ttu-id="d3c4f-102">Det här dokumentet går igenom hello skillnaderna mellan hanterade och ohanterade diskar när du använder Azure Resource Manager mallar tooprovision virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-102">This document walks through hello differences between managed and unmanaged disks when using Azure Resource Manager templates tooprovision virtual machines.</span></span> <span data-ttu-id="d3c4f-103">Detta hjälper tooupdate befintliga mallar som använder ohanterade diskar toomanaged diskar.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-103">This will help you tooupdate existing templates that are using unmanaged Disks toomanaged disks.</span></span> <span data-ttu-id="d3c4f-104">För referens anger vi använder hello [101-vm-enkel-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) mallen som en vägledning.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-104">For reference, we are using hello [101-vm-simple-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) template as a guide.</span></span> <span data-ttu-id="d3c4f-105">Du kan se hello mallen med hjälp av både [hanterade diskar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) och en tidigare version med hjälp av [ohanterad diskar](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) om du vill att toodirectly jämföra dem.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-105">You can see hello template using both [managed Disks](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) and a prior version using [unmanaged disks](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) if you'd like toodirectly compare them.</span></span>

## <a name="unmanaged-disks-template-formatting"></a><span data-ttu-id="d3c4f-106">Ohanterad diskar mallen formatering</span><span class="sxs-lookup"><span data-stu-id="d3c4f-106">Unmanaged Disks template formatting</span></span>

<span data-ttu-id="d3c4f-107">toobegin, vi ta en titt på hur ohanterade diskar har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-107">toobegin, we take a look at how unmanaged disks are deployed.</span></span> <span data-ttu-id="d3c4f-108">När du skapar en ohanterad diskar, måste en storage-konto toohold hello VHD-filer.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-108">When creating unmanaged disks, you need a storage account toohold hello VHD files.</span></span> <span data-ttu-id="d3c4f-109">Du kan skapa ett nytt lagringskonto eller använda en som redan finns.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-109">You can create a new storage account or use one that already exists.</span></span> <span data-ttu-id="d3c4f-110">Den här artikeln visar hur toocreate ett nytt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-110">This article will show you how toocreate a new storage account.</span></span> <span data-ttu-id="d3c4f-111">tooaccomplish, du behöver en lagringsresurs konto i hello resurser block enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-111">tooaccomplish this, you need a storage account resource in hello resources block as shown below.</span></span>

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

<span data-ttu-id="d3c4f-112">Inom hello virtuella datorobjektet behöver vi beroende hello storage-konto tooensure som har skapats innan hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-112">Within hello virtual machine object, we need a dependency on hello storage account tooensure that it's created before hello virtual machine.</span></span> <span data-ttu-id="d3c4f-113">Inom hello `storageProfile` avsnittet vi ange hello fullständiga URI: N för hello VHD-plats som refererar till hello storage-konto och krävs för hello OS-disk- och eventuella hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-113">Within hello `storageProfile` section, we then specify hello full URI of hello VHD location, which references hello storage account and is needed for hello OS disk and any data disks.</span></span> 

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

## <a name="managed-disks-template-formatting"></a><span data-ttu-id="d3c4f-114">Hanterade diskar mallen formatering</span><span class="sxs-lookup"><span data-stu-id="d3c4f-114">Managed disks template formatting</span></span>

<span data-ttu-id="d3c4f-115">Med Azure hanterade diskar hello disk blir en översta resurs och kräver inte längre en lagring konto toobe som skapats av hello användare.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-115">With Azure Managed Disks, hello disk becomes a top-level resource and no longer requires a storage account toobe created by hello user.</span></span> <span data-ttu-id="d3c4f-116">Hanterade diskar har först exponeras i hello `2016-04-30-preview` API-versionen som de är tillgängliga i alla efterföljande API-versioner och är nu hello standard disktyp.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-116">Managed disks were first exposed in hello `2016-04-30-preview` API version, they are available in all subsequent API versions and are now hello default disk type.</span></span> <span data-ttu-id="d3c4f-117">hello avsnitten gå igenom hello standardinställningar och innehåller information om hur toofurther anpassa diskarna.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-117">hello following sections walk through hello default settings and detail how toofurther customize your disks.</span></span>

> [!NOTE]
> <span data-ttu-id="d3c4f-118">Det rekommenderas toouse en API-version senare än `2016-04-30-preview` eftersom viktiga förändringar mellan `2016-04-30-preview` och `2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-118">It is recommended toouse an API version later than `2016-04-30-preview` as there were breaking changes between `2016-04-30-preview` and `2017-03-30`.</span></span>
>
>

### <a name="default-managed-disk-settings"></a><span data-ttu-id="d3c4f-119">Standardinställningarna för hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-119">Default managed disk settings</span></span>

<span data-ttu-id="d3c4f-120">toocreate en virtuell dator med hanterade diskar kan du inte längre behöver toocreate hello lagringsresurs konto och uppdatera din virtuella datorresursen enligt följande.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-120">toocreate a VM with managed disks, you no longer need toocreate hello storage account resource and can update your virtual machine resource as follows.</span></span> <span data-ttu-id="d3c4f-121">Observera att hello specifikt `apiVersion` återspeglar `2017-03-30` och hello `osDisk` och `dataDisks` inte längre finns tooa specifika URI för hello VHD.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-121">Specifically note that hello `apiVersion` reflects `2017-03-30` and hello `osDisk` and `dataDisks` no longer refer tooa specific URI for hello VHD.</span></span> <span data-ttu-id="d3c4f-122">När du distribuerar utan att ange ytterligare egenskaper hello disk kommer att använda [Standard LRS lagring](../articles/storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="d3c4f-122">When deploying without specifying additional properties, hello disk will use [Standard LRS storage](../articles/storage/common/storage-redundancy.md).</span></span> <span data-ttu-id="d3c4f-123">Om inget namn anges tar hello format `<VMName>_OsDisk_1_<randomstring>` för hello OS-disken och `<VMName>_disk<#>_<randomstring>` för varje datadisk.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-123">If no name is specified, it takes hello format of `<VMName>_OsDisk_1_<randomstring>` for hello OS disk and `<VMName>_disk<#>_<randomstring>` for each data disk.</span></span> <span data-ttu-id="d3c4f-124">Azure disk encryption är inaktiverat som standard. cachelagring är läsning och skrivning för hello OS-disk och ingen för datadiskar.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-124">By default, Azure disk encryption is disabled; caching is Read/Write for hello OS disk and None for data disks.</span></span> <span data-ttu-id="d3c4f-125">Du kan se i hello exemplet nedan har fortfarande ett beroende som storage-konto, även om detta är endast avsedd för lagring av diagnostik och behövs inte för disklagring.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-125">You may notice in hello example below there is still a storage account dependency, though this is only for storage of diagnostics and is not needed for disk storage.</span></span>

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

### <a name="using-a-top-level-managed-disk-resource"></a><span data-ttu-id="d3c4f-126">Med hjälp av en översta hanterade diskresurs</span><span class="sxs-lookup"><span data-stu-id="d3c4f-126">Using a top-level managed disk resource</span></span>

<span data-ttu-id="d3c4f-127">Du kan skapa en översta diskresurs och bifoga den som en del av hello virtuell dator skapas som ett alternativt toospecifying hello diskkonfigurationen i hello virtuella datorobjektet.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-127">As an alternative toospecifying hello disk configuration in hello virtual machine object, you can create a top-level disk resource and attach it as part of hello virtual machine creation.</span></span> <span data-ttu-id="d3c4f-128">Vi kan till exempel skapa en diskresurs enligt följande toouse som en datadisk.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-128">For example, we can create a disk resource as follows toouse as a data disk.</span></span>

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

<span data-ttu-id="d3c4f-129">Inom hello Virtuella datorobjektet referera vi sedan till den här disken objektet toobe ansluten.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-129">Within hello VM object, we can then reference this disk object toobe attached.</span></span> <span data-ttu-id="d3c4f-130">Att ange hello resurs-ID för hello hanteras disk som vi skapade i hello `managedDisk` egenskap kan hello ansluts hello disk som hello virtuell dator skapas.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-130">Specifying hello resource ID of hello managed disk we created in hello `managedDisk` property allows hello attachment of hello disk as hello VM is created.</span></span> <span data-ttu-id="d3c4f-131">Observera att hello `apiVersion` hello Virtuella datorresursen är inställd för`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-131">Note that hello `apiVersion` for hello VM resource is set too`2017-03-30`.</span></span> <span data-ttu-id="d3c4f-132">Tänk också på att vi har skapat ett beroende på hello disk resurs tooensure den har skapats innan du skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-132">Also note that we've created a dependency on hello disk resource tooensure it's successfully created before VM creation.</span></span> 

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

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a><span data-ttu-id="d3c4f-133">Skapa hanterade tillgänglighetsuppsättningar med virtuella datorer med hjälp av hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-133">Create managed availability sets with VMs using managed disks</span></span>

<span data-ttu-id="d3c4f-134">toocreate hanteras tillgänglighet uppsättningar med virtuella datorer med hjälp av hanterade diskar, lägga till hello `sku` objekt toohello tillgänglighet resurs och hello `name` egenskapen för`Aligned`.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-134">toocreate managed availability sets with VMs using managed disks, add hello `sku` object toohello availability set resource and set hello `name` property too`Aligned`.</span></span> <span data-ttu-id="d3c4f-135">Detta säkerställer att hello diskar för varje virtuell dator är tillräckligt isolerade från varandra tooavoid enskilda felpunkter.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-135">This ensures that hello disks for each VM are sufficiently isolated from each other tooavoid single points of failure.</span></span> <span data-ttu-id="d3c4f-136">Observera även att hello `apiVersion` för hello tillgänglighetsuppsättning resursen har angetts för`2017-03-30`.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-136">Also note that hello `apiVersion` for hello availability set resource is set too`2017-03-30`.</span></span>

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

### <a name="additional-scenarios-and-customizations"></a><span data-ttu-id="d3c4f-137">Fler scenarier och anpassningar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-137">Additional scenarios and customizations</span></span>

<span data-ttu-id="d3c4f-138">toofind fullständig information på REST API-specifikationer hello, granska hello [skapar du en hanterad disk REST API-dokumentation](/rest/api/manageddisks/disks/disks-create-or-update).</span><span class="sxs-lookup"><span data-stu-id="d3c4f-138">toofind full information on hello REST API specifications, please review hello [create a managed disk REST API documentation](/rest/api/manageddisks/disks/disks-create-or-update).</span></span> <span data-ttu-id="d3c4f-139">Du hittar fler scenarier, såväl som standard och godkända värden som kan vara skickade toohello API via mall-distributioner.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-139">You will find additional scenarios, as well as default and acceptable values that can be submitted toohello API through template deployments.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="d3c4f-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d3c4f-140">Next steps</span></span>

* <span data-ttu-id="d3c4f-141">Besök hello följande Azure Quickstart lagringsplatsen länkar för fullständig mallar som använder hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-141">For full templates that use managed disks visit hello following Azure Quickstart Repo links.</span></span>
    * [<span data-ttu-id="d3c4f-142">Windows virtuell dator med hanterad disk</span><span class="sxs-lookup"><span data-stu-id="d3c4f-142">Windows VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [<span data-ttu-id="d3c4f-143">Linux VM med hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-143">Linux VM with managed disk</span></span>](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [<span data-ttu-id="d3c4f-144">Fullständig lista över mallar för hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="d3c4f-144">Full list of managed disk templates</span></span>](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* <span data-ttu-id="d3c4f-145">Besök hello [hanterade diskar översikt över Azure](../articles/virtual-machines/windows/managed-disks-overview.md) dokument toolearn mer om hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-145">Visit hello [Azure Managed Disks Overview](../articles/virtual-machines/windows/managed-disks-overview.md) document toolearn more about managed disks.</span></span>
* <span data-ttu-id="d3c4f-146">Granska hello mallen i referensdokumentationen för virtuella datorresurser genom att besöka hello [Microsoft.Compute/virtualMachines mallreferensen](/templates/microsoft.compute/virtualmachines) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-146">Review hello template reference documentation for virtual machine resources by visiting hello [Microsoft.Compute/virtualMachines template reference](/templates/microsoft.compute/virtualmachines) document.</span></span>
* <span data-ttu-id="d3c4f-147">Granska hello mallen i referensdokumentationen för diskresurser genom att besöka hello [Microsoft.Compute/disks mallreferensen](/templates/microsoft.compute/disks) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="d3c4f-147">Review hello template reference documentation for disk resources by visiting hello [Microsoft.Compute/disks template reference](/templates/microsoft.compute/disks) document.</span></span>
 
