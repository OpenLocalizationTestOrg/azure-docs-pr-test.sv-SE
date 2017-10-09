# <a name="using-managed-disks-in-azure-resource-manager-templates"></a>Med hjälp av hanterade diskar i Azure Resource Manager-mallar

Det här dokumentet går igenom hello skillnaderna mellan hanterade och ohanterade diskar när du använder Azure Resource Manager mallar tooprovision virtuella datorer. Detta hjälper tooupdate befintliga mallar som använder ohanterade diskar toomanaged diskar. För referens anger vi använder hello [101-vm-enkel-windows](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows) mallen som en vägledning. Du kan se hello mallen med hjälp av både [hanterade diskar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows/azuredeploy.json) och en tidigare version med hjälp av [ohanterad diskar](https://github.com/Azure/azure-quickstart-templates/tree/93b5f72a9857ea9ea43e87d2373bf1b4f724c6aa/101-vm-simple-windows/azuredeploy.json) om du vill att toodirectly jämföra dem.

## <a name="unmanaged-disks-template-formatting"></a>Ohanterad diskar mallen formatering

toobegin, vi ta en titt på hur ohanterade diskar har distribuerats. När du skapar en ohanterad diskar, måste en storage-konto toohold hello VHD-filer. Du kan skapa ett nytt lagringskonto eller använda en som redan finns. Den här artikeln visar hur toocreate ett nytt lagringskonto. tooaccomplish, du behöver en lagringsresurs konto i hello resurser block enligt nedan.

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

Inom hello virtuella datorobjektet behöver vi beroende hello storage-konto tooensure som har skapats innan hello virtuell dator. Inom hello `storageProfile` avsnittet vi ange hello fullständiga URI: N för hello VHD-plats som refererar till hello storage-konto och krävs för hello OS-disk- och eventuella hårddiskar. 

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

## <a name="managed-disks-template-formatting"></a>Hanterade diskar mallen formatering

Med Azure hanterade diskar hello disk blir en översta resurs och kräver inte längre en lagring konto toobe som skapats av hello användare. Hanterade diskar har först exponeras i hello `2016-04-30-preview` API-versionen som de är tillgängliga i alla efterföljande API-versioner och är nu hello standard disktyp. hello avsnitten gå igenom hello standardinställningar och innehåller information om hur toofurther anpassa diskarna.

> [!NOTE]
> Det rekommenderas toouse en API-version senare än `2016-04-30-preview` eftersom viktiga förändringar mellan `2016-04-30-preview` och `2017-03-30`.
>
>

### <a name="default-managed-disk-settings"></a>Standardinställningarna för hanterade diskar

toocreate en virtuell dator med hanterade diskar kan du inte längre behöver toocreate hello lagringsresurs konto och uppdatera din virtuella datorresursen enligt följande. Observera att hello specifikt `apiVersion` återspeglar `2017-03-30` och hello `osDisk` och `dataDisks` inte längre finns tooa specifika URI för hello VHD. När du distribuerar utan att ange ytterligare egenskaper hello disk kommer att använda [Standard LRS lagring](../articles/storage/common/storage-redundancy.md). Om inget namn anges tar hello format `<VMName>_OsDisk_1_<randomstring>` för hello OS-disken och `<VMName>_disk<#>_<randomstring>` för varje datadisk. Azure disk encryption är inaktiverat som standard. cachelagring är läsning och skrivning för hello OS-disk och ingen för datadiskar. Du kan se i hello exemplet nedan har fortfarande ett beroende som storage-konto, även om detta är endast avsedd för lagring av diagnostik och behövs inte för disklagring.

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

### <a name="using-a-top-level-managed-disk-resource"></a>Med hjälp av en översta hanterade diskresurs

Du kan skapa en översta diskresurs och bifoga den som en del av hello virtuell dator skapas som ett alternativt toospecifying hello diskkonfigurationen i hello virtuella datorobjektet. Vi kan till exempel skapa en diskresurs enligt följande toouse som en datadisk.

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

Inom hello Virtuella datorobjektet referera vi sedan till den här disken objektet toobe ansluten. Att ange hello resurs-ID för hello hanteras disk som vi skapade i hello `managedDisk` egenskap kan hello ansluts hello disk som hello virtuell dator skapas. Observera att hello `apiVersion` hello Virtuella datorresursen är inställd för`2017-03-30`. Tänk också på att vi har skapat ett beroende på hello disk resurs tooensure den har skapats innan du skapa en virtuell dator. 

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

### <a name="create-managed-availability-sets-with-vms-using-managed-disks"></a>Skapa hanterade tillgänglighetsuppsättningar med virtuella datorer med hjälp av hanterade diskar

toocreate hanteras tillgänglighet uppsättningar med virtuella datorer med hjälp av hanterade diskar, lägga till hello `sku` objekt toohello tillgänglighet resurs och hello `name` egenskapen för`Aligned`. Detta säkerställer att hello diskar för varje virtuell dator är tillräckligt isolerade från varandra tooavoid enskilda felpunkter. Observera även att hello `apiVersion` för hello tillgänglighetsuppsättning resursen har angetts för`2017-03-30`.

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

### <a name="additional-scenarios-and-customizations"></a>Fler scenarier och anpassningar

toofind fullständig information på REST API-specifikationer hello, granska hello [skapar du en hanterad disk REST API-dokumentation](/rest/api/manageddisks/disks/disks-create-or-update). Du hittar fler scenarier, såväl som standard och godkända värden som kan vara skickade toohello API via mall-distributioner. 

## <a name="next-steps"></a>Nästa steg

* Besök hello följande Azure Quickstart lagringsplatsen länkar för fullständig mallar som använder hanterade diskar.
    * [Windows virtuell dator med hanterad disk](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-windows)
    * [Linux VM med hanterade diskar](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-simple-linux)
    * [Fullständig lista över mallar för hanterade diskar](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* Besök hello [hanterade diskar översikt över Azure](../articles/virtual-machines/windows/managed-disks-overview.md) dokument toolearn mer om hanterade diskar.
* Granska hello mallen i referensdokumentationen för virtuella datorresurser genom att besöka hello [Microsoft.Compute/virtualMachines mallreferensen](/templates/microsoft.compute/virtualmachines) dokumentet.
* Granska hello mallen i referensdokumentationen för diskresurser genom att besöka hello [Microsoft.Compute/disks mallreferensen](/templates/microsoft.compute/disks) dokumentet.
 
