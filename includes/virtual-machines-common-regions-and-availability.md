# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Regioner och tillgänglighet för virtuella datorer i Azure
Azure körs i flera Datacenter hello världen. Dessa Datacenter grupperas i toogeographic regioner, vilket ger dig möjlighet att välja var toobuild dina program. Det är viktigt toounderstand hur och var dina virtuella datorer (VM) fungerar i Azure, tillsammans med dina alternativ toomaximize prestanda, tillgänglighet och redundans. Den här artikeln ger en översikt över hello tillgänglighet och redundans funktioner i Azure.

## <a name="what-are-azure-regions"></a>Vad är Azure-regioner?
Du kan skapa Azure-resurser i definierade geografiska områden som 'Västra USA', 'Nordeuropa' eller 'Sydostasien'. Du kan granska hello [lista över regioner och deras platser](https://azure.microsoft.com/regions/). I varje region finns flera Datacenter tooprovide för redundans och tillgänglighet. Den här metoden ger flexibilitet när du utformar program toocreate VMs närmaste tooyour användare och toomeet eventuella, efterlevnad eller skatt syften.

## <a name="special-azure-regions"></a>Särskilda Azure-regioner
Azure har vissa särskilda områden att du toouse när du skapar ut dina program för kompatibilitet eller juridiska ändamål. De särskilda regionerna innefattar:

* **Virginia (USA-förvaltad region)** och **Iowa (USA-förvaltad region)**
  * En fysisk och logisk nätverksisolerad instans av Azure för amerikanska myndigheter och partner som drivs av säkerhetskontrollerad amerikansk personal. Innefattar ytterligare efterlevnadscertifieringar som [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) och [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Läs mer om [Azure Government](https://azure.microsoft.com/features/gov/).
* **Östra Kina** och **Norra Kina**
  * Dessa områden är tillgängliga via ett unikt partnerskap mellan Microsoft och 21Vianet, där Microsoft direkt ingen har hälsningspaket datacenter. Läs mer om [Microsoft Azure i Kina](http://www.windowsazure.cn/).
* **Centrala Tyskland** och **Nordöstra Tyskland**
  * Dessa områden är tillgängliga via en datamodell förvaltning där kunddata förblir i Tyskland under kontroll av T-system, en Deutsche Telekom företag som agerar som hello tyska data förvaltning.

## <a name="region-pairs"></a>Regionpar
Varje Azure-region paras ihop med en annan region inom hello samma geografi (till exempel USA, Europa eller Asien). Den här metoden ger för hello replikering av resurser, till exempel VM-lagring över en geografisk plats som ska minska hello sannolikheten för naturkatastrof civil unrest, strömavbrott eller fysiska nätverksavbrott påverkar både regioner på samma gång. Ytterligare fördelar med regionpar:

* I ett bredare Azure avbrott hello-händelse, en region prioriteras utanför varje par toohelp minska hello tid toorestore för program. 
* Planerade Azure uppdateringar är distribuerat toopaired regioner som vid en tidpunkt toominimize driftstopp och risken för programmet avbrott.
* Data fortsätter tooreside inom hello samma geografi som dess par (förutom södra) för skatt och lag tvingande behörighet.

Exempel på regionpar:

| Primär | Sekundär |
|:--- |:--- |
| Västra USA |Östra USA |
| Norra Europa |Västra Europa |
| Sydostasien |Östasien |

Du kan se hello fullständig [lista över regionala par här](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Funktionstillgänglighet
Vissa tjänster eller VM-funktioner är endast tillgängliga i vissa regioner, till exempel särskilda VM-storlekar eller lagringstyper. Det finns vissa globala Azure-tjänster som du inte behöver tooselect en viss region som [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md), eller [Azure DNS](../articles/dns/dns-overview.md). tooassist du utformar din miljö för programmet, du kan kontrollera hello [tillgängligheten för Azure-tjänster i varje region](https://azure.microsoft.com/regions/#services). 

## <a name="storage-availability"></a>Lagringstillgänglighet
Det är viktigt att förstå Azure-regioner och geografiska områden när du funderar hello replikeringsalternativ tillgängligt lagringsutrymme. Beroende på hello lagringstyp har du olika replikeringsalternativ.

**Azure Managed Disks**
* Lokalt redundant lagring (LRS)
  * Replikerar data tre gånger inom hello region där du skapade ditt lagringskonto.

**Diskar baserade på lagringskonto**
* Lokalt redundant lagring (LRS)
  * Replikerar data tre gånger inom hello region där du skapade ditt lagringskonto.
* Zonredundant lagring (ZRS)
  * Replikerar data tre gånger mellan två toothree verksamhet, i en enda region eller mellan två regioner.
* Geo-redundant lagring (GRS)
  * Replikeras dina data tooa sekundära region som är hundratals mil bort från hello primär region.
* Geo-redundant lagring med läsbehörighet (RA-GRS)
  * Replikeras dina data tooa sekundär region, precis som med GRS, men då också ger läsbehörighet toohello data på hello sekundär plats.

hello ger följande tabell en snabb överblick över hello skillnaderna mellan hello lagringstyper replikering:

| Replikeringsstrategi | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Data replikeras över flera anläggningar. |Nej |Ja |Ja |Ja |
| Data kan läsas från hello sekundär plats och hello primär plats. |Nej |Nej |Nej |Ja |
| Antal kopior av data som finns på olika noder. |3 |3 |6 |6 |

Du kan läsa mer om [Azure Storage-replikeringsalternativen här](../articles/storage/common/storage-redundancy.md). Mer information om hanterade diskar finns i [Översikt över Azure Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md).

### <a name="storage-costs"></a>Lagringskostnader
De priser som varierar beroende på hello lagringstyp och tillgänglighet som du väljer.

**Azure Managed Disks**
* Hanterade Premiumdiskar backas upp av Solid-State-hårddiskar (SSD) och hanteras standarddiskar backas upp av vanliga snurrande diskar. Premium- och hanteras standarddiskar debiteras baserat på hello etablerad kapacitet för hello disken.

**Ohanterade diskar**
* Premium-lagring backas upp av Solid-State-hårddiskar (SSD) och debiteras baserat på hello diskens hello kapacitet.
* Standardlagring backas upp av vanliga snurrande diskar och debiteras baserat på hello används kapacitet och vill lagerutrymme.
  * För RA-GRS kostar det en ytterligare dataöverföring för Geo-replikering för hello bandbredd med att replikera den data tooanother Azure-region.

Se [priser för Azure Storage](https://azure.microsoft.com/pricing/details/storage/) för prisuppgifter på hello olika lagringstyper och alternativ för tillgänglighet.

## <a name="availability-sets"></a>Tillgänglighetsuppsättningar
En tillgänglighetsuppsättning är en logisk gruppering av virtuella datorer som gör att Azure toounderstand hur programmet skapas tooprovide för redundans och tillgänglighet. Vi rekommenderar att två eller flera virtuella datorer skapas i en uppsättning tooprovide för tillgänglighet för en högtillgänglig program och toomeet hello [99,95% SLA för Azure](https://azure.microsoft.com/support/legal/sla/virtual-machines/). När en enda virtuell dator använder [Azure Premium Storage](../articles/storage/common/storage-premium-storage.md), hello Azure-serviceavtalet gäller för oplanerat underhållshändelser. 

En tillgänglighetsuppsättning består av två ytterligare grupperingar som skydd mot maskinvarufel och tillåta uppdateringar toosafely vara tillämpas - fault-domäner (FDs) och uppdatera domäner (UDs).

![Konceptuell ritning av hello domän och fel domän uppdateringskonfiguration](./media/virtual-machines-common-regions-and-availability/ud-fd-configuration.png)

Du kan läsa mer om hur toomanage hello tillgängligheten för [virtuella Linux-datorer](../articles/virtual-machines/linux/manage-availability.md) eller [virtuella Windows-datorer](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Feldomäner
En feldomän är en logisk grupp av underliggande maskinvara som delar en gemensam kraftkälla och nätverksswitch, liknande tooa rack inom ett lokalt datacenter. När du skapar virtuella datorer i en tillgänglighetsuppsättning distribuerar automatiskt dina virtuella datorer över dessa feldomäner hello Azure-plattformen. Den här metoden begränsar hello effekten av potentiella fysiska maskinvarufel, nätverksavbrott eller power avbrott.

### <a name="update-domains"></a>Uppdateringsdomäner
En uppdateringsdomän är en logisk grupp av underliggande maskinvara som kan genomgår underhåll eller startas om på hello samtidigt. När du skapar virtuella datorer i en tillgänglighetsuppsättning hello Azure-plattformen automatiskt distribuerar dina virtuella datorer mellan dessa uppdatera domäner. Den här metoden garanterar att minst en instans av programmet alltid körs som hello Azure-plattformen genomgår regelbundet underhåll. hello ordningen för update domäner startas inte kan fortsätta sekventiellt under planerat underhåll, men bara en uppdateringsdomän startas i taget.

### <a name="managed-disk-fault-domains"></a>Hanterade Disk feldomäner
För virtuella datorer som använder [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md) justeras de virtuella datorerna efter feldomänerna för hanterade diskar när en hanterad tillgänglighetsuppsättning används. Justeringen säkerställer att alla hello hanterade diskar anslutna tooa VM ligger inom hello samma feldomän för hanterade diskar. Endast virtuella datorer med hanterade diskar kan skapas i en hanterad tillgänglighetsuppsättning. hello antalet feldomäner för hanterade diskar varierar beroende på region - antingen två eller tre hanterade diskar feldomäner per region.

![Feldomäner med hanterade diskar](./media/virtual-machines-common-manage-availability/md-fd.png)

> [!IMPORTANT]
> hello antalet feldomäner för hanterade tillgänglighetsuppsättningar varierar beroende på region - två eller tre per region. hello visar följande tabell hello många per region

[!INCLUDE [managed-disks-common-fault-domain-region-list](managed-disks-common-fault-domain-region-list.md)]

## <a name="next-steps"></a>Nästa steg
Nu kan du starta toouse dessa tillgänglighet och redundans funktioner toobuild Azure-miljön. Metodtips hittar du i [Metodtips för Azure-tillgänglighet](../articles/best-practices-availability-checklist.md).

