# <a name="common-errors-during-classic-tooazure-resource-manager-migration"></a>Vanliga fel under klassisk tooAzure hanteraren för filserverresurser
Den här artikeln kataloger hello de vanligaste fel och åtgärder under hello migreringen av IaaS-resurser från klassiska Azure-distribution modellen toohello Azure Resource Manager-stacken.

## <a name="list-of-errors"></a>Lista över fel
| Felsträng | Åtgärd |
| --- | --- |
| Internt serverfel |I vissa fall är det här ett tillfälligt fel som försvinner vid ett nytt försök. Om det fortfarande toopersist, [kontakta Azure-supporten](../articles/azure-supportability/how-to-create-azure-support-request.md) behöver undersökning loggar plattform. <br><br> **Obs:** när hello incident spåras av hello supportteamet försök inte några egna minskning som detta kan ha oönskade konsekvenser på din miljö. |
| Migrering stöds inte för distributionen {deployment-name} i HostedService {hosted-service-name} eftersom det är en PaaS-distribution (Web/Worker). |Det här inträffar när en distribution innehåller en web/worker-roll. Eftersom migrering stöds bara för virtuella datorer, ta bort hello web/worker-rollen från hello distribution och försök migrera igen. |
| Det gick inte att distribuera mallen {template-name}. CorrelationId={guid} |I hello backend tjänsternas migrering använder vi Azure Resource Manager mallar toocreate resurser i hello Azure Resource Manager-stacken. Eftersom mallar är idempotent är försök vanligtvis på ett säkert sätt hello migrering åtgärden tooget tidigare felet. Om felet kvarstår toopersist [kontakta Azure-supporten](../articles/azure-supportability/how-to-create-azure-support-request.md) och ge dem hello CorrelationId. <br><br> **Obs:** när hello incident spåras av hello supportteamet försök inte några egna minskning som detta kan ha oönskade konsekvenser på din miljö. |
| hello virtuella nätverket {--namn för virtuellt nätverk} finns inte. |Detta kan inträffa om du har skapat hello virtuellt nätverk i hello nya Azure-portalen. hello faktiska virtuella nätverksnamnet följer hello mönster ”grupp * <VNET name>” |
| Den virtuella datorn {vm-name} i HostedService {hosted-service-name} innehåller tillägget {extension-name} som inte stöds i Azure Resource Manager. Det rekommenderas toouninstall från hello VM innan du fortsätter med migreringen. |XML-tillägg som BGInfo 1.* stöds inte i Azure Resource Manager. Därför kan de inte heller migreras. Om dessa tillägg är vänstra installeras på hello virtuell dator kan avinstalleras automatiskt innan du slutför hello migrering. |
| Den virtuella datorn {vm-name} i HostedService {hosted-service-name} innehåller tillägget VMSnapshot/VMSnapshotLinux, som för närvarande inte stöds för migrering. Avinstallera den från hello VM och Lägg till den igen med hjälp av Azure Resource Manager när hello migreringen är slutförd |Detta är hello scenario där hello virtuella datorn är konfigurerad för Azure Backup. Eftersom det är ett scenario som inte stöds, följ hello lösning på https://aka.ms/vmbackupmigration |
| VM {vm-name} i HostedService {värd-tjänst-name} innehåller tillägg {Tilläggsnamn} vars Status inte rapporteras från hello VM. Den virtuella datorn kan därför inte migreras. Kontrollera att hello tillståndets status rapporteras eller avinstallera hello tillägg från hello VM och försök migrering. <br><br> Den virtuella datorn {vm-name} i HostedService {hosted-service-name} innehåller tillägget {extension-name} som rapporterar hanterarstatus: {handler-status}. Därför kan inte migreras hello VM. Kontrollera att hello tillägget hanterare status har rapporterats är {hanterare status} eller avinstallera den från hello VM och försök migrering. <br><br> VM-agenten för den virtuella datorn {vm-name} i HostedService {värd-tjänst-name} rapporterar hello övergripande status för agent inte redo. Därför kan hello VM inte migreras, om den har filnamnstillägget maskinvaruegenskaperna. Se till att hello VM-agenten rapporterar övergripande status för agent som klar. Läs toohttps://aka.ms/classiciaasmigrationfaqs. |Azure gästagenten & VM-tillägg måste du utgående internet access toohello VM storage-konto toopopulate deras status. Vanliga orsaker till fel kan vara <li> en Nätverkssäkerhetsgrupp som blockerar utgående åtkomst toohello internet <li> Om hello VNET har lokala DNS-servrar och DNS-anslutningen bryts <br><br> Om du fortsätter toosee statusen som inte stöds kan du avinstallera hello tillägg tooskip kontrollen och gå vidare med migreringen. |
| Migrering stöds inte för distributionen {deployment-name} i HostedService {hosted-service-name} eftersom den har flera tillgänglighetsuppsättningar. |För närvarande går det bara att migrera värdbaserade tjänster med högst 1 tillgänglighetsuppsättning. toowork problemet, flytta hello ytterligare tillgänglighetsuppsättningar och virtuella datorer i dessa tillgänglighet anger tooa olika värdbaserade tjänsten. |
| Migrering stöds inte för distribution {distributionsnamnet} i HostedService {värd-tjänst-name eftersom den har virtuella datorer som inte är del av hello Tillgänglighetsuppsättningen även om hello HostedService innehåller en. |hello lösning för det här scenariot är tooeither flytta alla hello virtuella datorer i en enda tillgänglighet ange eller ta bort alla virtuella datorer från hello tillgänglighetsuppsättning i hello värdbaserade tjänsten. |
| Storage-konto/HostedService/virtuella nätverket {--namn för virtuellt nätverk} hello att migreras och därför ändras inte |Det här felet uppstår när hello ”Förbered”-migrering har slutförts på hello resursen och en åtgärd som gör en ändring toohello resurs utlöses. På grund av hello lock hello management plan efter ”Förbered” åtgärden, eventuella ändringar toohello resursen blockeras. toounlock hello management plan som du kan köra hello ”spara” migrering åtgärden toocomplete migrering eller hello ”Avbryt” migrering åtgärden tooroll tillbaka hello ”Förbered” igen. |
| Migrering tillåts inte för HostedService {hosted-service-name} eftersom den virtuella datorn {vm-name} har status: RoleStateUnknown. Migrering tillåts endast när hello virtuella datorn är i något av följande hello tillstånd - körs, Stoppad, stoppas frigjorts. |hello VM kan väldigt via en tillståndsövergång inträffar vanligtvis när under en uppdatering på hello HostedService, till exempel en omstart, installation av webbprogramtillägg osv. Det rekommenderas för hello uppdateringen igen toocomplete på hello HostedService innan du försöker migrera. |
| {Distributionsnamnet} innehåller i HostedService {värd-tjänst-name} en virtuell dator {vm-name} med datadisk {data-disknamnet} vars fysiska blob {size-of-the-vhd-blob-backing-the-data-disk} byte inte matchar hello VM datadisk logisk storlek { Size-of-the-data-disk-specified-in-the-VM-API} byte. Migrering fortsätter utan att ange en storlek för hello datadisk för hello Azure Resource Manager VM. | Det här felet uppstår om du har ändrat storlek hello VHD-blobben utan att uppdatera hello storlek i hello VM API-modellen. Utförliga anvisningar för migrering hittar du [nedan](#vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-the-vm-data-disk-logical-size-bytes).|
| Ett Storage-undantag uppstod under verifiering av datadisken {data disk name} med medielänken {data disk Uri} för den virtuella datorn {VM name} i molntjänsten {Cloud Service name}. Se till att hello VHD media-länk är tillgänglig för den virtuella datorn | Det här felet kan inträffa om hello diskar hello VM har tagits bort inte eller tillgänglig längre. Kontrollera att hello diskar för hello VM finns.|
| Den virtuella datorn {vm-name} i HostedService {cloud-service-name} innehåller en disk med MediaLink {vhd-uri} med blobbnamnet {vhd-blob-name} som inte stöds i Azure Resource Manager. | Felet uppstår när hello namnet på hello-blob har en ”/” i den som inte stöds för närvarande i Compute-Resursprovidern. |
| Migrering tillåts inte för distribution {distributionsnamnet} i HostedService {molntjänstnamnet} eftersom den inte är i hello regionala omfånget. Se toohttp://aka.ms/regionalscope för att flytta den här distributionen tooregional omfång. | Azure har 2014 meddelat att nätverksresurser flyttas från ett kluster nivån scope tooregional omfång. Mer information finns i [http://aka.ms/regionalscope] (http://aka.ms/regionalscope). Det här felet uppstår när hello distribution migreras inte har funnits en uppdateringsåtgärd som automatiskt flyttar tooa regionala omfånget. Bästa lösningen är tooeither lägga till en slutpunkt tooa VM eller en disk toohello VM och försök sedan migrering. <br> Se [hur tooset av slutpunkter för klassiska Windows-dator i Azure](../articles/virtual-machines/windows/classic/setup-endpoints.md#create-an-endpoint) eller [bifoga en data disk tooa Windows virtuell dator som skapats med hello klassiska distributionsmodellen](../articles/virtual-machines/windows/classic/attach-disk.md)|


## <a name="detailed-mitigations"></a>Detaljerade åtgärder

### <a name="vm-with-data-disk-whose-physical-blob-size-bytes-does-not-match-hello-vm-data-disk-logical-size-bytes"></a>Virtuell dator med datadisk vars fysiska blob-byte matchar inte hello VM datadisk logiska byte.

Detta händer när hello logiska datadiskstorleken får synkroniserad med hello faktiska VHD-blobbstorleken. Enkelt du kan kontrollera med hjälp av hello följande kommandon:

#### <a name="verifying-hello-issue"></a>Verifiera hello problemet

```PowerShell
# Store hello VM details in hello VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

# Display hello data disk properties
# NOTE hello data disk LogicalDiskSizeInGB below which is 11GB. Also note hello MediaLink Uri of hello VHD blob as we'll use this in hello next step
$vm.VM.DataVirtualHardDisks


HostCaching         : None
DiskLabel           : 
DiskName            : coreosvm-coreosvm-0-201611230636240687
Lun                 : 0
LogicalDiskSizeInGB : 11
MediaLink           : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
SourceMediaLink     : 
IOType              : Standard
ExtensionData       : 

# Now get hello properties of hello blob backing hello data disk above
# NOTE hello size of hello blob is about 15 GB which is different from LogicalDiskSizeInGB above
$blob = Get-AzureStorageblob -Blob "coreosvm-dd1.vhd" -Container vhds 

$blob

ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudPageBlob
BlobType          : PageBlob
Length            : 16106127872
ContentType       : application/octet-stream
LastModified      : 11/23/2016 7:16:22 AM +00:00
SnapshotTime      : 
ContinuationToken : 
Context           : Microsoft.WindowsAzure.Commands.Common.Storage.AzureStorageContext
Name              : coreosvm-dd1.vhd
```

#### <a name="mitigating-hello-issue"></a>Minimera hello problemet

```PowerShell
# Convert hello blob size in bytes tooGB into a variable which we'll use later
$newSize = [int]($blob.Length / 1GB)

# See hello calculated size in GB
$newSize

15

# Store hello disk name of hello data disk as we'll use this tooidentify hello disk toobe updated
$diskName = $vm.VM.DataVirtualHardDisks[0].DiskName

# Identify hello LUN of hello data disk tooremove
$lunToRemove = $vm.VM.DataVirtualHardDisks[0].Lun

# Now remove hello data disk from hello VM so that hello disk isn't leased by hello VM and it's size can be updated
Remove-AzureDataDisk -LUN $lunToRemove -VM $vm | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       213xx1-b44b-1v6n-23gg-591f2a13cd16   Succeeded  

# Verify we have hello right disk that's going toobe updated
Get-AzureDisk -DiskName $diskName

AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : 
Location             : East US
DiskSizeInGB         : 11
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 0c56a2b7-a325-123b-7043-74c27d5a61fd
OperationStatus      : Succeeded

# Now update hello disk toohello new size
Update-AzureDisk -DiskName $diskName -ResizedSizeInGB $newSize -Label $diskName

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureDisk     cv134b65-1b6n-8908-abuo-ce9e395ac3e7 Succeeded 

# Now verify that hello "DiskSizeInGB" property of hello disk matches hello size of hello blob 
Get-AzureDisk -DiskName $diskName


AffinityGroup        : 
AttachedTo           : 
IsCorrupted          : False
Label                : coreosvm-coreosvm-0-201611230636240687
Location             : East US
DiskSizeInGB         : 15
MediaLink            : https://contosostorage.blob.core.windows.net/vhds/coreosvm-dd1.vhd
DiskName             : coreosvm-coreosvm-0-201611230636240687
SourceImageName      : 
OS                   : 
IOType               : Standard
OperationDescription : Get-AzureDisk
OperationId          : 1v53bde5-cv56-5621-9078-16b9c8a0bad2
OperationStatus      : Succeeded

# Now we'll add hello disk back toohello VM as a data disk. First we need tooget an updated VM object
$vm = Get-AzureVM -ServiceName $servicename -Name $vmname

Add-AzureDataDisk -Import -DiskName $diskName -LUN 0 -VM $vm -HostCaching ReadWrite | Update-AzureVm -Name $vmname -ServiceName $servicename

OperationDescription OperationId                          OperationStatus
-------------------- -----------                          ---------------
Update-AzureVM       b0ad3d4c-4v68-45vb-xxc1-134fd010d0f8 Succeeded      
```

### <a name="moving-a-vm-tooa-different-subscription-after-completing-migration"></a>Flytta en virtuell dator tooa annan prenumeration när du har slutfört migreringen

När du har slutfört migreringen hello kanske toomove hello VM tooanother prenumeration. Men om du har en hemlighet /-certifikatet på stöds hello flytta VM som refererar till en Key Vault-resurs hello för närvarande inte. hello nedan instruktioner kan tooworkaround hello problemet. 

#### <a name="powershell"></a>PowerShell
```powershell
$vm = Get-AzureRmVM -ResourceGroupName "MyRG" -Name "MyVM"
Remove-AzureRmVMSecret -VM $vm
Update-AzureRmVM -ResourceGroupName "MyRG" -VM $vm
```
#### <a name="azure-cli-20"></a>Azure CLI 2.0

```bash
az vm update -g "myrg" -n "myvm" --set osProfile.Secrets=[]
```
