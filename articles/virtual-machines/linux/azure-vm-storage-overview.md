---
title: aaaAzure virtuella Linux-datorer och Azure Storage | Microsoft Docs
description: "Beskriver Azure Standard och Premium-lagring och både hanterade och ohanterade diskar med Linux-datorer."
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: d34441698a4e59721847685099e5fb3aa378c597
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-and-linux-vm-storage"></a>Azure-och Linux VM
Azure Storage är hello molnlagringslösningen för moderna program som förlitar sig på hållbarhet, tillgänglighet och skalbarhet toomeet hello kundernas behov.  I tillägg toomaking det möjligt för utvecklare toobuild storskaliga program toosupport nya scenarier, Azure Storage innehåller också hello lagringsgrunden för Azure Virtual Machines.

## <a name="managed-disks"></a>Managed Disks

Virtuella Azure-datorer är nu tillgängliga med [Azure hanterade diskar](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), vilket gör du toocreate dina virtuella datorer utan att skapa eller hantera alla [Azure Storage-konton](../../storage/common/storage-introduction.md) själv. Anger du om du vill använda Premium eller ska standardlagring och hur stor hello disk och Azure skapas hello Virtuella diskar. Virtuella datorer med hanterade diskar har många viktiga funktioner, inklusive:

- Stöd för automatisk skalbarhet. Azure skapar hello diskar och hanterar hello underliggande lagring toosupport in too10, 000 diskar per prenumeration.
- Ökar tillförlitligheten med Tillgänglighetsuppsättningar. Azure säkerställer att Virtuella diskar är isolerade från varandra i Tillgänglighetsuppsättningar automatiskt.
- Ökad åtkomstkontroll. Hanterade diskar exponera olika åtgärder som styrs av [rollbaserad åtkomstkontroll (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Priser för hanterade diskar skiljer sig för den ohanterade diskar. Den här informationen finns [priser och fakturering för hanterade diskar](../windows/managed-disks-overview.md#pricing-and-billing).

Du kan konvertera befintliga virtuella datorer som använder ohanterade diskar toouse hanterade diskar med [az vm konvertera](/cli/azure/vm#convert). Mer information finns i [hur tooconvert en Linux VM från ohanterade diskar tooAzure hanterade diskar](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Du kan inte konvertera en ohanterad disk till en hanterad disk om hello ohanterade disken är i ett lagringskonto som är, eller när som helst har, krypteras med [Azure Storage Service kryptering (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). hello Följ anvisningarna nedan för information om hur tootooconvert ohanterad diskar som är eller har gjorts i en krypterad storage-konto:

- Kopiera hello virtuell hårddisk (VHD) med [az storage blob kopiera start](/cli/azure/storage/blob/copy#start) tooa lagringskonto som aldrig har aktiverats för Azure Storage Service-kryptering.
- Skapa en virtuell dator som använder hanterade diskar och ange att VHD-filen när du skapar med [az vm skapa](/cli/azure/vm#create), eller
- Koppla hello kopieras VHD med [az vm disk bifoga](/cli/azure/vm/disk#attach) tooa kör virtuell dator med hanterade diskar.


## <a name="azure-storage-standard-and-premium"></a>Azure Storage: Standard och Premium
Virtuella Azure-datorer – om du använder hanterade diskar eller ohanterad--kan baseras på lagringsdiskar som standard eller premium-lagringsdiskar. När du använder hello portal toochoose den virtuella datorn måste du växla en listrutan på hello **grunderna** skärmen tooview både standard och premium-diskar. När toggled tooSSD, endast premium-lagring som är aktiverade virtuella datorer visas alla backas upp av SSD-enheter.  När toggled tooHDD, standard-storage-aktiverade virtuella datorer som backas upp av centrifugering diskenheter som visas, tillsammans med premium-lagring VMs backas upp av SSD.

När du skapar en virtuell dator från hello `azure-cli` du kan välja mellan standard och premium när du väljer hello VM-storlek via hello `-z` eller `--vm-size` cli-flaggan.

## <a name="creating-a-vm-with-a-managed-disk"></a>Skapa en virtuell dator med en hanterad Disk

hello följande exempel kräver hello Azure CLI 2.0, vilket du kan [installera här](/cli/azure/install-azure-cli).

Börja med att skapa en resurs grupp toomanage hello resurser med [az gruppen skapa](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Nu skapa hello virtuell dator med [az vm skapa](/cli/azure/vm#create). Ange ett unikt `--public-ip-address-dns-name` argument som `mypublicdns` utförs troligen.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

hello föregående exempel skapar en virtuell dator med en hanterad disk i ett standardlagringskonto. toouse ett premiumlagringskonto lägga till hello `--storage-sku Premium_LRS` argumentet som hello följande exempel:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Standard Storage
Azure Storage som Standard är hello standardtyp av lagring.  Standardlagring är kostnadseffektiv samtidigt som de performant.  

## <a name="premium-storage"></a>Premium Storage
Azure Premium Storage ger stöd för virtuella datorer som körs I/O-intensiva arbetsbelastningar diskar med hög prestanda, låg latens. Virtuell dator (VM)-diskar som använder Premium-lagring kan du lagra data på solid state-hårddiskar (SSD). Du kan migrera programmets Virtuella diskar tooAzure Premium-lagring tootake nytta av hello hastighet och prestanda för dessa diskar.

Premium-lagringsfunktioner:

* Premium-lagringsdiskar: Azure Premium Storage har stöd för Virtuella diskar som kan vara anslutna tooDS, DSv2 eller GS-serien Azure virtuella datorer.
* Premium Sidblob: Premium-lagring stöder Azure Sidblobbar som används toohold beständiga diskar för virtuella Azure-datorer (VM).
* Premium lokalt Redundant lagring: Ett Premium Storage-konto bara har stöd för lokalt Redundant lagring (LRS) som hello replikeringsalternativet och fortsätter tre kopior av hello data inom en enskild region.
* [Premium-lagring](../../storage/common/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium-lagring stöds virtuella datorer
Premium-lagring stöder DS-serien, DSv2-serien GS-serien och Fs-serien Azure virtuella datorer (VM). Du kan använda Standard- och Premium diskar med lagringsutrymme med Premium-lagring som stöds av virtuella datorer. Men du kan inte använda Premium-lagring diskar med VM-serien, som inte är kompatibel Premium-lagring.

Följande är hello Linux-distributioner som vi verifieras med Premium-lagring.

| Distribution | Version | Stöds Kernel |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Azure File storage
Azure File storage erbjuder filresurser i hello molnet med hello SMB-standardprotokollet. Du kan migrera företagsprogram som förlitar sig på filen servrar tooAzure med Azure-filer. Program som körs i Azure kan enkelt montera filresurser från Azure virtuella datorer som kör Linux. Och med hello senaste versionen av fillagring, du kan också montera en filresurs från ett lokalt program som stöder SMB 3.0.  Eftersom filresurser är SMB-resurser, kan du komma åt dem via standardfilsystemet API: er.

Fillagring bygger på hello samma teknik som Blob-, tabell- och kön lagring, så File storage erbjuder hello tillgänglighet, hållbarhet, skalbarhet och geo-redundans som är inbyggd i hello Azure storage-plattformen. Mer information om prestandamål för File storage och begränsningar finns i Azure Storage skalbarhets- och prestandamål.

* [Hur toouse Azure File storage med Linux](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Frekvent
hello Azure frekventa lagringsnivå är optimerad för att lagra data som används ofta.  Frekvent är hello lagring standardtypen för blob-Arkiv.

## <a name="cool-storage"></a>Lågfrekvent
hello Azure lågfrekvent åtkomstnivå är optimerad för att lagra data som används ofta och långlivade. Exempel på användningsområden för lågfrekvent innehåller säkerhetskopior, medieinnehåll, vetenskapliga data, efterlevnad och arkiveringsdata. I allmänhet är alla data som används sällan en perfekt kandidat för lågfrekvent.

|  | Frekvent lagringsnivå | Lågfrekvent lagringsnivå |
|:--- |:---:|:---:|
| Tillgänglighet |99,9 % |99 % |
| Tillgänglighet (RA-GRS-läsningar) |99,99 % |99,9 % |
| Avgifter för användning |Högre kostnader för lagring |Lägre kostnader för lagring |
| Lägre åtkomst |Högre åtkomst | |
| och transaktionskostnader |och transaktionskostnader | |

## <a name="redundancy"></a>Redundans
hello data i din Microsoft Azure storage-konto är alltid replikeras tooensure hållbarhet och hög tillgänglighet, uppfyller hello Azure Storage SLA även i hello sida av tillfälliga maskinvarufel.

När du skapar ett lagringskonto måste du välja ett av följande replikeringsalternativ hello:

* Lokalt redundant lagring (LRS)
* Zonredundant lagring (ZRS)
* Geo-redundant lagring (GRS)
* Geo-redundant lagring med läsbehörighet (RA-GRS)

### <a name="locally-redundant-storage"></a>Lokalt redundant lagring
Lokalt redundant lagring (LRS) replikerar data inom hello region där du skapade ditt lagringskonto. toomaximize hållbarhet alla begäranden som görs mot data i ditt lagringskonto replikeras tre gånger. Dessa tre repliker finns i separata feldomäner och uppgraderingsdomäner.  En begäran returnerar har endast när det har skrivits tooall tre repliker.

### <a name="zone-redundant-storage"></a>Zonredundant lagring
Zonredundant lagring (ZRS) replikerar data mellan två toothree anläggningar, antingen i en enda region eller mellan två regioner, vilket ger högre hållbarhet än LRS. Om ditt lagringskonto har ZRS aktiverat, sedan dina data skyddas även i hello fall av fel på någon av hello lokaler.

### <a name="geo-redundant-storage"></a>Geografiskt redundant lagring.
GEO-redundant lagring (GRS) replikeras dina data tooa sekundära region som är hundratals mil bort från hello primär region. Om ditt lagringskonto har GRS aktiverat, sedan dina data skyddas även i hello fallet med ett komplett regionalt strömavbrott eller en katastrofåterställning i vilka hello primär region inte kan återställas.

### <a name="read-access-geo-redundant-storage"></a>Geo-redundant lagring med läsbehörighet
Geo-redundant lagring med läsbehörighet (RA-GRS) maximerar tillgänglighet för ditt lagringskonto med läsbehörighet toohello data i hello sekundär plats, dessutom toohello replikering mellan två regioner som tillhandahålls av GRS. I händelse av hello data blir otillgängliga i hello primära region, kan ditt program läsa data från hello sekundär region.

För en djupdykning i Azure storage redundans finns:

* [Azure Storage-replikering](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Skalbarhet
Azure Storage är extremt skalbart, så att du kan lagra och bearbeta hundratals terabyte data toosupport hello stordatascenarier krävs av vetenskapsprogram, ekonomisk analys, medieprogram och program. Eller så kan du lagra hello små mängder data som krävs för en liten företagswebbplats. Oavsett dina behov, betalar bara för hello data du lagrar. Azure Storage lagrar för närvarande flera biljoner unika kundobjekt och hanterar många miljoner förfrågningar per sekund i genomsnitt.

För standardlagring konton: ett standardlagringskonto har en maximal Totalt antal förfrågningar av 20 000 IOPS. hello får totala IOPS för alla virtuella diskar i ett standardlagringskonto inte överskrida den här gränsen.

För premium storage-konton: ett premiumlagringskonto har en maximal totala genomflödet andel 50 Gbit/s. hello totala genomströmningen för alla Virtuella diskar får inte överskrida den här gränsen.

## <a name="availability"></a>Tillgänglighet
Vi garanterar att minst 99,99% (99,9% för åtkomstnivån cool) hello tid, vi har ska bearbeta begäranden tooread data från läsbehörighet Geo-Redundant lagring (RA-GRS)-konton, förutsatt att misslyckades försök tooread data från hello primära regionen är göra på hello sekundär region.

Vi garanterar att minst 99,9% (99% för åtkomstnivån cool) hello tid, vi kommer att processen begär tooread data från lokalt Redundant lagring (LRS), zonen Redundant lagring (ZRS) och konton för Geo-Redundant lagring (GRS).

Vi garanterar att minst 99,9% (99% för åtkomstnivån cool) hello tid, vi ska kunna process begär toowrite data tooLocally Redundant lagring (LRS), zonen Redundant lagring (ZRS), och Geo-Redundant lagring (GRS) konton och läsbehörighet Geo Redundant Lagringskonton (RA-GRS).

* [Azure SLA för Storage](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Regioner
Azure är allmänt tillgänglig i 30 regioner runt hello world och har tillkännagivit planer för 4 ytterligare regioner. Geografisk expansion är en prioritet för Azure eftersom det ger våra kunder tooachieve högre prestanda och den stöder sina krav och inställningar för plats.  Azures senaste region toolaunch är i Tyskland.

hello Microsoft Cloud Tyskland ger en differentierad alternativet toohello Microsoft Cloud-tjänsterna är redan tillgänglig över Europa, skapa större möjligheter för innovation och tillväxt för hög reglerade partner och kunder i Tyskland hello Europeiska unionen (EU) och hello Europeiska associationen (EFTA).

Kundinformation i dessa nya datacenter i Magdeburg och Frankfurt, hanteras under hello kontroll av en data-förvaltare, T-Systems internationella, ett oberoende tyska företag och dotterbolag till Deutsche Telekom. Microsofts kommersiella molntjänster i dessa Datacenter följa tooGerman föreskrifter för datahantering och ge kunderna ytterligare alternativ för hur och var data bearbetas.

* [Azure-regioner karta](https://azure.microsoft.com/regions/)

## <a name="security"></a>Säkerhet
Azure Storage tillhandahåller en omfattande uppsättning säkerhetsfunktioner som tillsammans ger utvecklare möjligheten toobuild säkra program. Hej lagringskonto själva kan skyddas med hjälp av rollbaserad åtkomstkontroll och Azure Active Directory. Data kan skyddas vid överföring mellan en program- och Azure med hjälp av kryptering på klientsidan, HTTPS och SMB 3.0. Data kan ställas in toobe krypteras automatiskt när skrivas tooAzure lagring med hjälp av kryptering för lagring-tjänsten (SSE). Operativsystemet och datadiskarna som används av virtuella datorer kan ställas in toobe som krypterats med Azure Disk Encryption. Delegerad åtkomst toohello dataobjekt i Azure Storage kan tilldelas använder signaturer för delad åtkomst.

### <a name="management-plane-security"></a>Hantering av plan säkerhet
hello management plan som består av hello resurser används toomanage ditt lagringskonto. I det här avsnittet kommer vi hello Azure Resource Manager-distributionsmodellen och hur toouse rollbaserad åtkomstkontroll (RBAC) toocontrol åt tooyour storage-konton. Kommer dessutom pratar vi om hur du hanterar din lagringskontonycklar och hur tooregenerate dem.

### <a name="data-plane-security"></a>Säkerhet för data-plan
I det här avsnittet ska vi titta på att tillåta åtkomst toohello faktiska dataobjekt i ditt lagringskonto, till exempel blobbar, filer, köer och tabeller som använder signaturer för delad åtkomst och åtkomstregler lagras. Vi tar upp både servicenivåer SAS och konto-nivå SAS. Vi också se hur toolimit åt tooa specifik IP-adress (eller intervall av IP-adresser), hur toolimit hello protokoll som används tooHTTPS och hur toorevoke en signatur för delad åtkomst utan att vänta på den tooexpire.

## <a name="encryption-in-transit"></a>Kryptering under överföring
Det här avsnittet beskrivs hur toosecure data vid överföring till eller från Azure Storage. Lär dig om hello rekommenderar användning av HTTPS och hello kryptering som används av SMB 3.0 för Azure-filresurser. Vi kommer också ta en titt på klientsidan kryptering, vilket gör att du tooencrypt hello data innan det överförs till lagring i ett klientprogram och toodecrypt hello data när det överförs utanför lagring.

## <a name="encryption-at-rest"></a>Kryptering i vila
Vi pratar om Storage Service-kryptering (SSE) och hur du kan aktivera det för ett lagringskonto, vilket resulterar i din blockblobbar, sidblobbar, och tilläggsblobar krypteras automatiskt när skrivas tooAzure lagring. Vi kommer också titta på hur du kan använda Azure Disk Encryption och utforska hello grundläggande skillnader och fall av diskkryptering jämfört med SSE jämfört med kryptering på klientsidan. Vi ser kort på FIPS-kompatibilitet för USA Offentliga datorer.

* [Azure Storage-säkerhetsguiden](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Diskutrymme
Varje virtuell dator innehåller en tillfällig disk. hello diskutrymme tillhandahåller kortsiktig lagring för program och processer och är avsedd tooonly lagra data, till exempel page eller växlingen filer. Data på hello diskutrymme kan gå förlorade under en [underhållshändelse](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) eller när du [distribuera en virtuell dator](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Under en standard omstart av hello VM ska hello data på hello temporärkatalog sparas.

På virtuella Linux-datorer hello disken är vanligtvis **/dev/sdb** har formaterats och monterade för**/mnt** av hello Azure Linux-agenten. hello storleken på hello diskutrymme varierar utifrån hello storleken på hello virtuella datorn. Mer information finns i [storlekar för virtuella Linux-datorer](sizes.md).

Mer information om hur Azure använder hello diskutrymme finns [förstå hello tillfälliga enheten på virtuella datorer i Microsoft Azure](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Besparingar
* [Kostnad för datalagring](https://azure.microsoft.com/pricing/details/storage/)
* [Lagringsberäknaren kostnad](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Lagringsgränser
* [Lagringsgränser för tjänsten](../../azure-subscription-service-limits.md#storage-limits)
