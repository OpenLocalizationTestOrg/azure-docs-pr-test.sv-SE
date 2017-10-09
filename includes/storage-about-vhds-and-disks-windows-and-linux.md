
## <a name="about-vhds"></a>Om virtuella hårddiskar

hello virtuella hårddiskar som används i Azure är VHD-filer som lagras som sidblobbar i en standard- eller premium storage-konto i Azure. Mer information om sidblobar finns [Understanding block blobs and page blobs](/rest/api/storageservices/Understanding-Block-Blobs--Append-Blobs--and-Page-Blobs/) (Förstå blockblobar och sidblobar). Mer information om premium-lagring finns i [High-performance premium storage and Azure VMs](../articles/storage/common/storage-premium-storage.md) (Premium-lagring och virtuella Azure-datorer med hög prestanda).

Azure stöder hello fast disk VHD-format. hello fast format skapar hello logisk disk ut linjärt i hello-fil, så att disken förskjutningen X lagras på blob-offset X. En liten sidfot hello slutet av hello blob beskriver hello egenskaper för hello VHD. Ofta slösar hello fast format utrymme eftersom de flesta diskar med stora oanvända intervall i. Dock Azure lagrar VHD-filer i en sparse-format så får du hello fördelarna med båda hello fast och dynamiska diskar på hello samma tid. Mer information finns i [Komma igång med virtuella hårddiskar](https://technet.microsoft.com/library/dd979539.aspx).

Alla VHD-filer i Azure som du vill toouse som en källa toocreate diskar eller avbildningar är skrivskyddade. När du skapar en disk eller avbildning gör Azure kopior av hello VHD-filer. Dessa kopior kan vara skrivskyddade eller och-skrivskyddad, beroende på hur du använder hello VHD.

När du skapar en virtuell dator från en avbildning skapar Azure en disk för hello virtuell dator som är en kopia av hello käll-VHD-filen. tooprotect oavsiktlig borttagning Azure placerar ett lån på alla käll-VHD-filen som har använt toocreate en bild, en operativsystemdisk eller en datadisk.

Innan du kan ta bort en käll-VHD-filen måste tooremove hello lån genom att ta bort hello disk eller avbildningen. toodelete en VHD-fil som används av en virtuell dator som en operativsystemdisk, du kan ta bort hello virtuell dator och hello operativsystemdisk hello käll-VHD-filen på en gång genom att ta bort hello virtuella datorn och ta bort alla associerade diskar. Men det krävs att du genomför ett antal steg i en viss ordning för att ta bort en .vhd-fil som är en källa för en datadisk. Först du koppla från hello disk från hello virtuell dator och sedan ta bort hello disken och ta sedan bort hello VHD-filen.

> [!WARNING]
> Om du tar bort en .vhd-fil som används som källa från lagringen eller tar bort ditt lagringskonto kan Microsoft inte återställa dessa data åt dig.
> 

## <a name="types-of-disks"></a>Typer av diskar 

Azure-diskar har en tillförlitlighet på 99,999 %. Azure-diskarna har konsekvent leverera företagsklass hållbarhet med ett branschledande noll % Felintervall Annualized.

Det finns två prestandanivåer för lagring som du kan välja när du skapar diskar – Standard Storage och Premium Storage. Dessutom finns två typer av diskar, ohanterade och hanterade, som kan användas på båda prestandanivåerna.


### <a name="standard-storage"></a>Standard Storage 

Standard Storage stöds av hårddiskar och levererar kostnadseffektiv lagring samtidigt som det är högpresterande. Standard Storage kan replikeras lokalt i ett datacenter eller vara geo-redundant med primära och sekundära datacenter. Mer information om lagringsreplikeringsalternativ finns i [Azure Storage-replikering](../articles/storage/common/storage-redundancy.md). 

Mer information om hur du använder Standard Storage med VM-diskar finns i [Standard Storage and Disks](../articles/storage/common/storage-standard-storage.md) (Standard Storage och diskar).

### <a name="premium-storage"></a>Premium Storage 

Premium Storage stöds av solid state-hårddiskar och levererar högpresterande disksupport med låg fördröjning för virtuella datorer som kör I/O-intensiva arbetsbelastningar. Du kan använda Premium-lagring med DS, DSv2, GS, Ls eller FS serien Azure virtuella datorer. Mer information finns i [Premium Storage](../articles/storage/common/storage-premium-storage.md).

### <a name="unmanaged-disks"></a>Ohanterade diskar

Ohanterad diskar är hello vanlig typ av diskar som har använts av virtuella datorer. Med dessa kan du skapa egna storage-konto och ange detta lagringskonto när du skapar hello disk. Du har för många diskar inte placera toomake hello samma lagringskonto, eftersom du kan överskrida hello [skalbarhetsmål](../articles/storage/common/storage-scalability-targets.md) av hello lagringskonto (20 000 IOPS, till exempel), vilket resulterar i hello VMs begränsas. Med ohanterad diskar har toofigure ut hur toomaximize hello användning av en eller flera konton tooget hello bästa lagringsprestanda utanför din virtuella dator.

### <a name="managed-disks"></a>Hanterade diskar 

Hanterade diskar handtag hello lagring utgör skapande och hantering i hello bakgrund och garanterar att du inte har tooworry om hello skalbarhetsbegränsningar av hello storage-konto. Du bara ange hello diskstorleken och hello prestandanivån (Standard/Premium) och Azure skapar och hanterar hello disk åt dig. Även om du lägger till diskar eller skala upp eller ned hello VM, kan du inte har tooworry om hello lagring som används. 

Du kan också hantera egna, anpassade avbildningar i ett lagringskonto per Azure-region och använda dem toocreate hundratals för virtuella datorer i hello samma prenumeration. Mer information om hanterade diskar finns hello [översikt för hanterade diskar](../articles/virtual-machines/windows/managed-disks-overview.md).

Vi rekommenderar att du använder Azure hanterade diskar för nya virtuella datorer och att du konverterar tidigare ohanterade diskar toomanaged diskarna, tootake nytta av hello många funktioner som är tillgängliga i hanterade diskar.

### <a name="disk-comparison"></a>Diskjämförelse

hello följande tabell innehåller en jämförelse av Premium vs Standard för både ohanterade och hanterade diskar toohelp du bestämma vilka toouse.

|    | Azure Premium-disk | Azure Standard-disk |
|--- | ------------------ | ------------------- |
| Disktyp | Solid State-hårddiskar (SSD) | Hårddiskar (HDD)  |
| Översikt  | SSD-baserad högpresterande disksupport med låg fördröjning för virtuella datorer som kör I/O-intensiva arbetsbelastningar eller är värd för verksamhetskritisk produktionsmiljö | HDD-baserad kostnadseffektiv disksupport för Dev/Test-VM-scenarier |
| Scenario  | Produktion och prestandakänsliga arbetsbelastningar | Dev/Test, icke-kritiska, <br>Lågfrekvent åtkomst |
| Diskstorlek | P4: 32 GB (endast för hanterade diskar)<br>P6: 64 GB (endast för hanterade diskar)<br>P10: 128 GB<br>P20: 512 GB<br>P30: 1024 GB<br>P40: 2 048 GB<br>P 50: 4095 GB | Ohanterad diskar: 1 GB – 4 TB (4095 GB) <br><br>Hanterade diskar:<br> S4: 32 GB <br>S6: 64 GB <br>S10: 128 GB <br>S20: 512 GB <br>S30: 1024 GB <br>S40: 2 048 GB<br>S50: 4095 GB| 
| Maxdataflöde per disk | 250 MB/s | 60 MB/s | 
| Max-IOPS per disk | 7500 IOPS | 500 IOPS | 

