## <a name="overview"></a>Översikt
När du skapar en ny virtuell dator (VM) i en resursgrupp genom att distribuera en avbildning från [Azure Marketplace](https://azure.microsoft.com/marketplace/), hello standardenheten OS är 127 GB. Även om det är möjligt tooadd data diskar toohello VM (hur många beroende på hello SKU du har valt) och dessutom är det rekommenderade tooinstall program och CPU-intensiv arbetsbelastning på diskarna tillägg, måste ofta kunder tooexpand hello OS enheten toosupport vissa scenarier, till exempel följande:

1. Stöd för äldre program som installerar komponenter på operativsystemenheten.
2. Migrera en fysisk eller virtuell dator från lokal plats med större operativsystemenhet.

> [!IMPORTANT]
> I Azure finns två olika distributionsmodeller för att skapa och arbeta med resurser: Resource Manager och klassisk. Den här artikeln täcker hello Resource Manager-modellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.
> 
> 

## <a name="resize-hello-os-drive"></a>Ändra storlek på hello OS-enhet
I den här artikeln ska vi utföra hello uppgiften för storleksändring hello OS-enhet med hjälp av resource manager moduler [Azure Powershell](/powershell/azureps-cmdlets-docs). Öppna din Powershell ISE eller Powershell-fönster i administrativa läge och hello följande sätt:

1. Logga in tooyour Microsoft Azure konto i resursen hanteringsläge och välja din prenumeration på följande sätt:
   
   ```Powershell
   Login-AzureRmAccount
   Select-AzureRmSubscription –SubscriptionName 'my-subscription-name'
   ```
2. Ange namn på resursgrupp och virtuell dator på följande sätt:
   
   ```Powershell
   $rgName = 'my-resource-group-name'
   $vmName = 'my-vm-name'
   ```
3. Hämta en referens tooyour VM på följande sätt:
   
   ```Powershell
   $vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```
4. Stoppa hello VM innan du ändrar storlek på hello disk på följande sätt:
   
    ```Powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
    ```
5. Och här kommer hello ögonblick vi har väntat på! Ange hello storleken på hello OS toohello önskad värde och uppdatera hello VM enligt följande:
   
   ```Powershell
   $vm.StorageProfile.OSDisk.DiskSizeGB = 1023
   Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
   ```
   
   > [!WARNING]
   > hello nya storleken måste vara större än hello befintliga diskens storlek. hello maximalt tillåtna är 1 023 GB.
   > 
   > 
6. Uppdaterar hello VM kan ta några sekunder. När hello-kommandot har slutförts körs, startas hello VM på följande sätt:
   
   ```Powershell
   Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
   ```

Klart! Nu RDP till hello VM, öppna Datorhantering (eller Diskhantering) och expandera hello-enhet med hello nyligen allokerat utrymme.

## <a name="summary"></a>Sammanfattning
Vi använde Azure Resource Manager moduler Powershell tooexpand hello OS-enhet på en virtuell IaaS-dator i den här artikeln. Nedan är hello fullständiga skriptet referens:

```Powershell
Login-AzureRmAccount
Select-AzureRmSubscription -SubscriptionName 'my-subscription-name'
$rgName = 'my-resource-group-name'
$vmName = 'my-vm-name'
$vm = Get-AzureRmVM -ResourceGroupName $rgName -Name $vmName
Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName
$vm.StorageProfile.OSDisk.DiskSizeGB = 1023
Update-AzureRmVM -ResourceGroupName $rgName -VM $vm
Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
```

## <a name="next-steps"></a>Nästa steg
Men i den här artikeln fokuserar huvudsakligen på expanderande hello VM hello OS-disk, kan hello utvecklade skript även användas för att expandera hello data diskar anslutna toohello VM genom att ändra en enda rad kod. Till exempel tooexpand hello först data disk bifogade toohello VM, Ersätt hello ```OSDisk``` objekt av ```StorageProfile``` med ```DataDisks``` matrisen och använda ett numeriskt index tooobtain referens toofirst bifogade datadisk, enligt nedan:

```Powershell
$vm.StorageProfile.DataDisks[0].DiskSizeGB = 1023
```
På samma sätt kan du referera till andra data diskar anslutna toohello VM, antingen med hjälp av ett index som visas ovan eller hello ```Name``` -egenskapen för hello disk som på bilden nedan:

```Powershell
($vm.StorageProfile.DataDisks | Where {$_.Name -eq 'my-second-data-disk'})[0].DiskSizeGB = 1023
```

Om du vill att toofind ut hur tooattach diskar tooan Azure Resource Manager VM, markerar du det [artikel](../articles/virtual-machines/windows/attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

