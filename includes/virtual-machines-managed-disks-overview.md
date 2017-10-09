# <a name="azure-managed-disks-overview"></a>Översikt över Azure-hanterade diskar

Azure-hanterade diskar förenklar Diskhantering för virtuella Azure IaaS-datorer genom att hantera hello [lagringskonton](../articles/storage/common/storage-introduction.md) som är associerade med hello Virtuella diskar. Du har bara toospecify hello typ ([Premium](../articles/storage/common/storage-premium-storage.md) eller [Standard](../articles/storage/common/storage-standard-storage.md)) och hello storleken på disk och Azure skapar och hanterar hello disk du.

## <a name="benefits-of-managed-disks"></a>Fördelarna med hanterade diskar

Låt oss ta en titt på vissa av hello fördelar får du med hjälp av hanterade diskar från och med den här Channel 9 videon [bättre Azure VM återhämtning med hanterade diskar](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Enkel och skalbar distribution av Virtuella datorer

Hanterade diskar handtag lagring du hello bakgrunden. Tidigare var du tvungen toocreate konton toohold hello lagringsdiskar (VHD-filer) för din virtuella Azure-datorer. När du ökar, var du tvungen att du har skapat ytterligare lagringskonton, så att du inte överskrider hello IOPS gränsen för lagring med alla diskar toomake. För hanterade diskar hantering lagring, är du inte längre begränsad hello lagringskontogränser (till exempel 20 000 IOPS / -kontot). Du har också längre toocopy anpassade avbildningar (VHD-filer) toomultiple storage-konton. Du kan hantera dem på en central plats – ett lagringskonto per Azure-region – och använder dem toocreate hundratals för virtuella datorer i en prenumeration.

Hanterade diskar kan du toocreate in too10 000 VM **diskar** för en prenumeration som gör att du toocreate tusentalsavgränsare av **VMs** i en enda prenumeration. Den här funktionen dessutom ytterligare ökar hello skalbarhet [virtuella skala uppsättningar (VMSS)](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) genom att låta dig toocreate upp tooa tusen virtuella datorer i en VMSS med hjälp av en Marketplace-avbildning.

### <a name="better-reliability-for-availability-sets"></a>Bättre tillförlitlighet för Tillgänglighetsuppsättningar

Hanterade diskar ger bättre tillförlitlighet för Tillgänglighetsuppsättningar genom att säkerställa att hello diskar [virtuella datorer i en Tillgänglighetsuppsättning](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) är tillräckligt isolerade från varandra tooavoid enskilda felpunkter. Detta sker automatiskt placerar hello diskar i olika skalningsenheter (stämplar). Om en stämpel misslyckas på grund av toohardware-eller programvarufel, inte bara hello VM-instanser med diskar på de stämplarna. Till exempel anta att du har ett program som körs på fem virtuella datorer och hello virtuella datorer finns i en Tillgänglighetsuppsättning. hello lagras diskar för dessa virtuella datorer inte alla i samma stämpel hello så att om en stämpel kraschar hello andra instanser av programmet hello toorun.

### <a name="highly-durable-and-available"></a>Extremt tillförlitliga och tillgängliga

Azure-diskar har en tillförlitlighet på 99,999 %. REST-lättare att veta att du har tre kopior av dina data som möjliggör hög hållbarhet. Om problem uppstår i en eller två även repliker säkerställer hello återstående repliker persistence för dina data och hög tolerans mot fel. Tack vare den här arkitekturen har Azure oavbrutet kunnat tillhandahålla tillförlitlighet på storföretagsnivå för sina IaaS-diskar. Azure är branschledande inom detta område med 0 % driftstopp per år. 

### <a name="granular-access-control"></a>Detaljerad åtkomstkontroll

Du kan använda [rollbaserad åtkomstkontroll (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) tooassign specifika behörigheter för en tooone för hanterade diskar eller fler användare. Hanterade diskar visar olika åtgärder, inklusive läsa, skriva (skapa/uppdatera), ta bort och hämtar en [signatur för delad åtkomst (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) för hello disken. Du kan bevilja åtkomst tooonly hello operations en person behöver tooperform sitt jobb. Om du inte vill att en person toocopy ett lagringskonto för hanterade diskar tooa, kan du välja inte toogrant åtkomst toohello export åtgärd för den hantera disken. På samma sätt om du inte vill att en person toouse en SAS-URI-toocopy hanterade diskar kan du inte toogrant som behörighet toohello hanterade diskar.

### <a name="azure-backup-service-support"></a>Stöd för Azure Backup service
Använda Azure Backup service med hanterade diskar toocreate en säkerhetskopiering med tidsbaserade säkerhetskopieringar, enkelt VM-återställning och säkerhetskopiering bevarandeprinciper. Hanterade diskar stöder endast lokalt Redundant lagring (LRS) som hello replikeringsalternativet; Det innebär att den bevarar tre kopior av hello data inom en enskild region. För regional katastrofåterställning, måste du säkerhetskopiera din Virtuella diskar i en annan region med hjälp av [Azure Backup service](../articles/backup/backup-introduction-to-azure-backup.md) och ett GRS-lagringskonto som säkerhetskopieringsvalvet. Stöder för närvarande Azure Backup data diskstorlekar in too1TB för säkerhetskopiering. Läs mer om detta i [med hjälp av Azure Backup-tjänsten för virtuella datorer med hanterade diskar](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Priser och fakturering

När du använder hanterade diskar gäller hello efter fakturering överväganden:
* Lagringstyp

* Diskstorlek

* Antal transaktioner

* Utgående dataöverföringar

* Hanterade diskbilder (fullständig kopia)

Låt oss ta en närmare titt på dessa.

**Lagringstyp:** hanterade diskar erbjuder 2 prestandanivåer: [Premium](../articles/storage/common/storage-premium-storage.md) (SSD-baserad) och [Standard](../articles/storage/common/storage-standard-storage.md) (HDD-baserat). hello fakturering för hanterade diskar beror på vilken typ av lagring som du har valt för hello disken.


**Diskstorlek**: fakturering för hanterade diskar beror på hello etablerats hello diskens storlek. Azure maps hello etablerade storlek (avrunda uppåt) toohello närmsta hanterade diskar alternativet som anges i hello tabellerna nedan. Varje hanterade diskar maps tooone av hello stöds etablerade storlekar och därefter faktureras. Om du skapar en standard hanterade diskar och ange en etablerade storlek på 200 GB debiteras du till exempel enligt hello prissättningen av hello S20 disktyp.

Här följer hello diskstorlekar för en hanterad premium-disk:

| **Premium hanteras <br>disktyp** | **P4** | **P6** |**P10** | **P20** | **P30** | **P40** | **P 50** | 
|------------------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Diskstorlek        | 32 GB   | 64 GB   | 128 GB  | 512 GB  | 1 024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 


Här följer hello diskstorlekar för en standard hanterade disk:

| **Standard hanteras <br>disktyp** | **S4** | **S6** | **S10** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|----------------|----------------|----------------| 
| Diskstorlek        | 32 GB   | 64 GB   | 128 GB | 512 GB | 1 024 GB (1 TB) | 2 048 GB (2 TB) | 4095 GB (4 TB) | 


**Antal transaktioner**: du debiteras för hello antal transaktioner som du kan utföra på en standard hanterade disk. Det kostar inget för transaktioner för en hanterad premium-disk.

**Utgående dataöverföringar**: [utgående dataöverföringar](https://azure.microsoft.com/pricing/details/data-transfers/) (data skickas från Azure-datacenter) debiteras för bandbreddsanvändning.

Detaljerad information om priser för hanterade diskar finns [hanterade diskar priser](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Hanterade diskbilder

En hanterad ögonblicksbild är en skrivskyddad fullständig kopia av en hanterad disk som lagras som standard hanterade disk som standard. Med ögonblicksbilder, kan du säkerhetskopiera hanterade diskar när som helst i tid. Dessa ögonblicksbilder finns oberoende av hello källdisken och kan vara används toocreate nya hanterade diskar. De debiteras baserat på hello används storlek. Om du skapar en ögonblicksbild av en hanterad disk med etablerad kapacitet 64 GB och storleken för data som används på 10 GB, till exempel debiteras ögonblicksbild endast för hello används datastorleken på 10 GB.  

[Inkrementell ögonblicksbilder](../articles/virtual-machines/windows/incremental-snapshots.md) stöds inte för närvarande för hanterade diskar, men kommer att stödjas i framtida hello.

toolearn mer om hur toocreate ögonblicksbilder för hanterade diskar, finns följande resurser:

* [Skapa kopia av en virtuell hårddisk som lagras som en hanterad disk med hjälp av ögonblicksbilder i Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Skapa kopia av en virtuell hårddisk som lagras som en hanterad disk med hjälp av ögonblicksbilder i Linux](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)


## <a name="images"></a>Avbildningar

Hanterade diskar också stöd för att skapa en anpassad hanterad avbildning. Du kan skapa en avbildning från din anpassade virtuella hårddiskar i ett lagringskonto eller direkt från en generaliserad (sys prepped) VM. Detta samlar in i en enda avbildning samtliga hanterade diskar som är kopplad till en virtuell dator, inklusive både hello OS- och datadiskar. Detta gör det möjligt att skapa virtuella datorer med hjälp av den anpassade avbildningen utan hello hundratals måste toocopy eller hantera storage-konton.

Information om att skapa avbildningar finns på hello följande artiklar:
* [Hur toocapture en hanterad avbildning av en generaliserad virtuell dator i Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Hur toogeneralize och avbilda en Linux virtuella datorer med hjälp av hello Azure CLI 2.0](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Bilder jämfört med ögonblicksbilder

Du ser ofta hello ordet ”bild” användes med virtuella datorer, men nu ”ögonblicksbilder” samt. Det är viktigt toounderstand hello skillnaden mellan dessa. För hanterade diskar, kan du ta en bild av en generaliserad virtuell dator som har frigjorts. Den här avbildningen innehåller alla hello diskar anslutna toohello VM. Du kan använda den här avbildningen toocreate en ny virtuell dator och den innehåller alla hello diskar.

En ögonblicksbild är en kopia av en disk på hello punkt i hämtas. Gäller endast tooone disk. Om du har en virtuell dator som bara har en disk (hello OS) kan du skapa en virtuell dator från hello ögonblicksbild eller hello avbildningen ta en ögonblicksbild eller en avbildning av den.

Vad händer om en virtuell dator har fem diskar och de stripe? Du kan ta en ögonblicksbild av varje hello diskar, men det finns inga medvetenhet inom hello VM av hello hello diskar – hello ögonblicksbilder veta endast om en disk. I det här fallet hello ögonblicksbilder måste toobe samordnas med varandra och som inte stöds.

## <a name="managed-disks-and-encryption"></a>Hanterade diskar och kryptering

Det finns två typer av kryptering toodiscuss i referens toomanaged diskar. hello först är en Storage Service kryptering (SSE), som utförs av hello storage-tjänst. hello är andra Azure Disk Encryption, där du kan aktivera på hello OS- och datadiskar för dina virtuella datorer.

### <a name="storage-service-encryption-sse"></a>Storage Service-kryptering (SSE)

[Azure Storage Service-kryptering](../articles/storage/common/storage-service-encryption.md) tillhandahåller kryptering i vila och skydda dina data toomeet din organisations säkerhet och efterlevnad åtaganden. SSE är aktiverat som standard för alla hanterade diskar, ögonblicksbilder och bilder i alla hello regioner där hanterade diskar är tillgänglig. Startar den 10 juni 2017 samtliga nya hanterade diskar-ögonblicksbilder-avbildningar och nya data skrivs tooexisting hanterade diskar är automatiskt krypterat i vila med nycklar som hanteras av Microsoft.  Besök hello [hanterade diskar vanliga frågor om sidan](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) för mer information.


### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

Azure Disk Encryption kan du tooencrypt hello OS- och datadiskar som används av en virtuell IaaS-dator. Detta omfattar hanterade diskar. För Windows krypteras hello-enheter med BitLocker-kryptering branschstandard. För Linux krypteras hello diskar med hello DM-Crypt teknik. Detta är integrerad med Azure Key Vault tooallow du toocontrol och hantera hello disk krypteringsnycklar. Mer information finns [Azure Disk Encryption för Windows och Linux IaaS-VM](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Nästa steg

Mer information om hanterade diskar finns toohello följande artiklar.

### <a name="get-started-with-managed-disks"></a>Kom igång med Managed Disks

* [Skapa en virtuell dator med Resource Manager och PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Skapa en Linux VM som använder hello Azure CLI 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

* [Koppla en virtuell Windows-dator med hanterad data disk tooa med hjälp av PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Lägg till en disk hanterade tooa Linux VM](../articles/virtual-machines/linux/add-disk.md)

* [Hanterade diskar PowerShell-exempelskript](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Använda hanterade diskar i Azure Resource Manager-mallar](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Jämför lagringsalternativ för hanterade diskar

* [Premium-lagring och diskar](../articles/storage/common/storage-premium-storage.md)

* [Standardlagring och diskar](../articles/storage/common/storage-standard-storage.md)

### <a name="operational-guidance"></a>Driftvägledning

* [Migrera från AWS och andra plattformar tooManaged diskar i Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Konvertera virtuella datorer i Azure toomanaged diskar i Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
